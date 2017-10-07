---
title: "Oktatóanyag: Azure Active Directoryval integrált HappyFox |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és HappyFox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 1190652d7d1144c7eddea339cb3f9175912407fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="5ab30-103">Oktatóanyag: Azure Active Directoryval integrált HappyFox</span><span class="sxs-lookup"><span data-stu-id="5ab30-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="5ab30-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HappyFox az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5ab30-104">In this tutorial, you learn how toointegrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5ab30-105">HappyFox integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5ab30-105">Integrating HappyFox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5ab30-106">Megadhatja a hozzáférés tooHappyFox rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5ab30-106">You can control in Azure AD who has access tooHappyFox</span></span>
- <span data-ttu-id="5ab30-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHappyFox (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5ab30-107">You can enable your users tooautomatically get signed-on tooHappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5ab30-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5ab30-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5ab30-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5ab30-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ab30-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5ab30-110">Prerequisites</span></span>

<span data-ttu-id="5ab30-111">az Azure AD integrálása HappyFox tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5ab30-111">tooconfigure Azure AD integration with HappyFox, you need hello following items:</span></span>

- <span data-ttu-id="5ab30-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5ab30-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5ab30-113">Egy HappyFox egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="5ab30-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5ab30-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5ab30-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5ab30-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5ab30-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5ab30-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5ab30-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5ab30-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ab30-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5ab30-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5ab30-118">Scenario description</span></span>
<span data-ttu-id="5ab30-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5ab30-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5ab30-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5ab30-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5ab30-121">Hello gyűjteményből HappyFox hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5ab30-121">Adding HappyFox from hello gallery</span></span>
2. <span data-ttu-id="5ab30-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5ab30-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-hello-gallery"></a><span data-ttu-id="5ab30-123">Hello gyűjteményből HappyFox hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5ab30-123">Adding HappyFox from hello gallery</span></span>
<span data-ttu-id="5ab30-124">tooconfigure hello integrációja HappyFox az Azure AD-be, meg kell tooadd HappyFox hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5ab30-124">tooconfigure hello integration of HappyFox into Azure AD, you need tooadd HappyFox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5ab30-125">**tooadd HappyFox hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5ab30-125">**tooadd HappyFox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ab30-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5ab30-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5ab30-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5ab30-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5ab30-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5ab30-133">Hello keresési mezőbe, írja be a **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-133">In hello search box, type **HappyFox**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="5ab30-135">A hello eredmények panelen válassza ki a **HappyFox**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-135">In hello results panel, select **HappyFox**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5ab30-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5ab30-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5ab30-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján HappyFox</span><span class="sxs-lookup"><span data-stu-id="5ab30-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5ab30-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó HappyFox tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5ab30-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HappyFox is tooa user in Azure AD.</span></span> <span data-ttu-id="5ab30-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello HappyFox közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5ab30-140">In other words, a link relationship between an Azure AD user and hello related user in HappyFox needs toobe established.</span></span>

<span data-ttu-id="5ab30-141">HappyFox, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5ab30-141">In HappyFox, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5ab30-142">tooconfigure és az Azure AD az egyszeri bejelentkezés HappyFox-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5ab30-142">tooconfigure and test Azure AD single sign-on with HappyFox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5ab30-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5ab30-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5ab30-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ab30-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5ab30-145">**[HappyFox tesztfelhasználó létrehozása](#creating-a-happyfox-test-user)**  -toohave egy megfelelője a Britta Simon a HappyFox, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5ab30-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - toohave a counterpart of Britta Simon in HappyFox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5ab30-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5ab30-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5ab30-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5ab30-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5ab30-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ab30-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5ab30-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az HappyFox alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5ab30-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="5ab30-150">**az Azure AD tooconfigure egyszeri bejelentkezést a HappyFox, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5ab30-150">**tooconfigure Azure AD single sign-on with HappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ab30-151">Az Azure portál, a hello hello **HappyFox** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-151">In hello Azure portal, on hello **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5ab30-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5ab30-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="5ab30-155">A hello **HappyFox tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5ab30-155">On hello **HappyFox Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="5ab30-157">a.</span><span class="sxs-lookup"><span data-stu-id="5ab30-157">a.</span></span> <span data-ttu-id="5ab30-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="5ab30-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="5ab30-159">b.</span><span class="sxs-lookup"><span data-stu-id="5ab30-159">b.</span></span> <span data-ttu-id="5ab30-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="5ab30-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5ab30-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="5ab30-161">These values are not real.</span></span> <span data-ttu-id="5ab30-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="5ab30-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5ab30-163">Ügyfél [HappyFox ügyfél-támogatási csoport](https://support.happyfox.com/home) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5ab30-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) tooget these values.</span></span> 
 
4. <span data-ttu-id="5ab30-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5ab30-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="5ab30-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5ab30-168">A hello **HappyFox konfigurációs** kattintson **konfigurálása HappyFox** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5ab30-168">On hello **HappyFox Configuration** section, click **Configure HappyFox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5ab30-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="5ab30-171">Tooyour HappyFox személyzet portál aláírásához, és keresse meg a túl**kezelése**, kattintson a **integrációja** fülre.</span><span class="sxs-lookup"><span data-stu-id="5ab30-171">Sign on tooyour HappyFox staff portal and navigate too**Manage**, Click on **Integrations** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="5ab30-173">Hello Integrációk lapon kattintson **konfigurálása** alatt **SAML-integráció** tooopen hello egyetlen bejelentkezést a beállítások.</span><span class="sxs-lookup"><span data-stu-id="5ab30-173">In hello Integrations tab, Click **Configure** under **SAML Integration** tooopen hello Single Sign On Settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="5ab30-175">SAML-alapú konfigurációs szakaszon belül illessze be a hello **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure portálról másolt **SSO célként megadott URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5ab30-175">Inside SAML configuration section, paste hello **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="5ab30-177">Nyissa meg a Jegyzettömbben az Azure portálról letöltött hello tanúsítványt, és illessze be a benne lévő tartalom **IdP aláírás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5ab30-177">Open hello certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="5ab30-179">Kattintson a **beállítások mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-179">Click **Save Settings** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="5ab30-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5ab30-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5ab30-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5ab30-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5ab30-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5ab30-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5ab30-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ab30-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="5ab30-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5ab30-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5ab30-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5ab30-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ab30-188">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5ab30-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5ab30-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ab30-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5ab30-194">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5ab30-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5ab30-196">a.</span><span class="sxs-lookup"><span data-stu-id="5ab30-196">a.</span></span> <span data-ttu-id="5ab30-197">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5ab30-198">b.</span><span class="sxs-lookup"><span data-stu-id="5ab30-198">b.</span></span> <span data-ttu-id="5ab30-199">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5ab30-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5ab30-200">c.</span><span class="sxs-lookup"><span data-stu-id="5ab30-200">c.</span></span> <span data-ttu-id="5ab30-201">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5ab30-202">d.</span><span class="sxs-lookup"><span data-stu-id="5ab30-202">d.</span></span> <span data-ttu-id="5ab30-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="5ab30-204">HappyFox tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ab30-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="5ab30-205">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="5ab30-205">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5ab30-206">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5ab30-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5ab30-207">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHappyFox megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5ab30-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHappyFox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5ab30-209">**tooassign Britta Simon tooHappyFox, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5ab30-209">**tooassign Britta Simon tooHappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ab30-210">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5ab30-212">Hello alkalmazások listában válassza ki a **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-212">In hello applications list, select **HappyFox**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="5ab30-214">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5ab30-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5ab30-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ab30-216">Click **Add** button.</span></span> <span data-ttu-id="5ab30-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ab30-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5ab30-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5ab30-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5ab30-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ab30-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5ab30-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ab30-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5ab30-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5ab30-222">Testing single sign-on</span></span>

<span data-ttu-id="5ab30-223">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5ab30-223">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="5ab30-224">Ha a hozzáférési Panel hello hello HappyFox csempe gombra kattint, HappyFox alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="5ab30-224">When you click hello HappyFox tile in hello Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="5ab30-225">Megtekintheti az hello **"SAML"** gombra hello bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="5ab30-225">You should see hello **‘SAML’** button on hello sign-in page.</span></span>

    ![Beépülő modul](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="5ab30-227">Kattintson a hello **"SAML"** gomb toolog a tooHappyFox az Azure AD-fiókot használ.</span><span class="sxs-lookup"><span data-stu-id="5ab30-227">Click hello **‘SAML’** button toolog in tooHappyFox using your Azure AD account.</span></span>

<span data-ttu-id="5ab30-228">További információ a hozzáférési Panel hello: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5ab30-228">For more information about hello Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5ab30-229">További források</span><span class="sxs-lookup"><span data-stu-id="5ab30-229">Additional resources</span></span>

* [<span data-ttu-id="5ab30-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5ab30-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5ab30-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5ab30-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

