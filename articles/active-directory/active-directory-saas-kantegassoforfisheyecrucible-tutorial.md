---
title: "Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a FishEye/tégelyt |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a FishEye/tégelyt Kantega SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: fdd68b5e90c3e2893887650735429a33e21ffa68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a><span data-ttu-id="55b36-103">Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a FishEye/tégelyt</span><span class="sxs-lookup"><span data-stu-id="55b36-103">Tutorial: Azure Active Directory integration with Kantega SSO for FishEye/Crucible</span></span>

<span data-ttu-id="55b36-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kantega egyszeri bejelentkezés az Azure Active Directory (Azure AD) FishEye/szálakat.</span><span class="sxs-lookup"><span data-stu-id="55b36-104">In this tutorial, you learn how toointegrate Kantega SSO for FishEye/Crucible with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="55b36-105">A FishEye/tégelyt Kantega SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="55b36-105">Integrating Kantega SSO for FishEye/Crucible with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="55b36-106">Megadhatja a hozzáférés tooKantega SSO rendelkező FishEye/tégelyt az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="55b36-106">You can control in Azure AD who has access tooKantega SSO for FishEye/Crucible</span></span>
- <span data-ttu-id="55b36-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKantega SSO vonatkozóan FishEye/tégelyt (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="55b36-107">You can enable your users tooautomatically get signed-on tooKantega SSO for FishEye/Crucible (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="55b36-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="55b36-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="55b36-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="55b36-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55b36-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55b36-110">Prerequisites</span></span>

<span data-ttu-id="55b36-111">tooconfigure Kantega SSO FishEye/tégelyt az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="55b36-111">tooconfigure Azure AD integration with Kantega SSO for FishEye/Crucible, you need hello following items:</span></span>

- <span data-ttu-id="55b36-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="55b36-112">An Azure AD subscription</span></span>
- <span data-ttu-id="55b36-113">Egy Kantega SSO FishEye/tégelyt egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="55b36-113">A Kantega SSO for FishEye/Crucible single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="55b36-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="55b36-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="55b36-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="55b36-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="55b36-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="55b36-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="55b36-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="55b36-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="55b36-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="55b36-118">Scenario description</span></span>
<span data-ttu-id="55b36-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="55b36-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="55b36-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="55b36-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="55b36-121">A FishEye/tégelyt Kantega SSO hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="55b36-121">Adding Kantega SSO for FishEye/Crucible from hello gallery</span></span>
2. <span data-ttu-id="55b36-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55b36-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-fisheyecrucible-from-hello-gallery"></a><span data-ttu-id="55b36-123">A FishEye/tégelyt Kantega SSO hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="55b36-123">Adding Kantega SSO for FishEye/Crucible from hello gallery</span></span>
<span data-ttu-id="55b36-124">tooconfigure hello integrációja a FishEye/tégelyt Kantega SSO az Azure AD-be, szükség van tooadd Kantega SSO FishEye/tégelyt hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="55b36-124">tooconfigure hello integration of Kantega SSO for FishEye/Crucible into Azure AD, you need tooadd Kantega SSO for FishEye/Crucible from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="55b36-125">**tooadd Kantega SSO FishEye/tégelyt hello gyűjteményből a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55b36-125">**tooadd Kantega SSO for FishEye/Crucible from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="55b36-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55b36-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="55b36-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="55b36-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="55b36-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55b36-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="55b36-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="55b36-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="55b36-133">Hello keresési mezőbe, írja be a **Kantega SSO FishEye/tégelyt a**.</span><span class="sxs-lookup"><span data-stu-id="55b36-133">In hello search box, type **Kantega SSO for FishEye/Crucible**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

5. <span data-ttu-id="55b36-135">A hello eredmények panelen válassza a **Kantega SSO FishEye/tégelyt a**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-135">In hello results panel, select **Kantega SSO for FishEye/Crucible**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="55b36-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55b36-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="55b36-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a FishEye/tégelyt "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="55b36-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for FishEye/Crucible based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="55b36-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kantega SSO FishEye/tégelyt a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="55b36-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kantega SSO for FishEye/Crucible is tooa user in Azure AD.</span></span> <span data-ttu-id="55b36-140">Ez azt jelenti hello Kantega SSO FishEye/tégelyt a kapcsolódó felhasználó és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="55b36-140">In other words, a link relationship between an Azure AD user and hello related user in Kantega SSO for FishEye/Crucible needs toobe established.</span></span>

<span data-ttu-id="55b36-141">FishEye/tégelyt Kantega egyszeri Bejelentkezést, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="55b36-141">In Kantega SSO for FishEye/Crucible, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="55b36-142">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel FishEye/tégelyt, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="55b36-142">tooconfigure and test Azure AD single sign-on with Kantega SSO for FishEye/Crucible, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="55b36-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="55b36-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="55b36-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55b36-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="55b36-145">**[Egy Kantega SSO FishEye/tégelyt teszt felhasználó létrehozása](#creating-a-kantega-sso-for-fisheyecrucible-test-user)**  -toohave Britta Simon a Kantega egyszeri Bejelentkezést a felhasználói csatolt toohello az Azure AD ábrázolása FishEye/tégelyt valami.</span><span class="sxs-lookup"><span data-stu-id="55b36-145">**[Creating a Kantega SSO for FishEye/Crucible test user](#creating-a-kantega-sso-for-fisheyecrucible-test-user)** - toohave a counterpart of Britta Simon in Kantega SSO for FishEye/Crucible that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="55b36-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55b36-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="55b36-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="55b36-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="55b36-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="55b36-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="55b36-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Kantega SSO FishEye/tégelyt alkalmazáshoz az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="55b36-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kantega SSO for FishEye/Crucible application.</span></span>

<span data-ttu-id="55b36-150">**tooconfigure az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a FishEye/tégelyt, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55b36-150">**tooconfigure Azure AD single sign-on with Kantega SSO for FishEye/Crucible, perform hello following steps:**</span></span>

1. <span data-ttu-id="55b36-151">Az Azure portál, a hello hello **Kantega SSO FishEye/tégelyt a** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="55b36-151">In hello Azure portal, on hello **Kantega SSO for FishEye/Crucible** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="55b36-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55b36-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

3. <span data-ttu-id="55b36-155">A **IDP** mód, a hello kezdeményezett **Kantega SSO FishEye/tégelyt tartomány és az URL-címek** szakasz hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="55b36-155">In **IDP** initiated mode, on hello **Kantega SSO for FishEye/Crucible Domain and URLs** section perform hello following step :</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    <span data-ttu-id="55b36-157">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-157">a.</span></span> <span data-ttu-id="55b36-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="55b36-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="55b36-159">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-159">b.</span></span> <span data-ttu-id="55b36-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="55b36-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="55b36-161">A **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="55b36-161">In **SP** initiated mode, check **Show advanced URL settings** and perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    <span data-ttu-id="55b36-163">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="55b36-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="55b36-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="55b36-164">These values are not real.</span></span> <span data-ttu-id="55b36-165">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="55b36-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="55b36-166">Ezek az értékek fogadásának FishEye/tégelyt beépülő modul hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="55b36-166">These values are recieved during hello configuration of FishEye/Crucible plugin which is explained later in hello tutorial.</span></span>

5. <span data-ttu-id="55b36-167">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="55b36-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

6. <span data-ttu-id="55b36-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="55b36-171">Egy másik webes böngészőablakban jelentkezzen be a helyi kiszolgálón FishEye/tégelyt tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55b36-171">In a different web browser window, log in tooyour FishEye/Crucible on premise server as an administrator.</span></span>

8. <span data-ttu-id="55b36-172">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="55b36-172">Hover on cog and click hello **Add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon1.png)

9. <span data-ttu-id="55b36-174">Rendszerbeállítások szakaszban kattintson **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="55b36-174">Under System Settings section, click **Find new add-ons**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/add-on2.png)

10. <span data-ttu-id="55b36-176">Keresési **Kantega SSO tégelyt a** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="55b36-176">Search **Kantega SSO for Crucible** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon2.png)

11. <span data-ttu-id="55b36-178">hello beépülő modul telepítésének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="55b36-178">hello plugin installation starts.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon33.png)

12. <span data-ttu-id="55b36-180">Hello telepítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="55b36-180">Once hello installation is complete.</span></span> <span data-ttu-id="55b36-181">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-181">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon34.png)

13. <span data-ttu-id="55b36-183">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-183">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon35.png)

14. <span data-ttu-id="55b36-185">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="55b36-185">Click **Configure** tooconfigure hello new plugin.</span></span>  

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon3.png)

15. <span data-ttu-id="55b36-187">A hello **SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="55b36-187">In hello **SAML** section.</span></span> <span data-ttu-id="55b36-188">Válassza ki **Azure Active Directory (Azure AD)** a hello **Hozzáadás identitásszolgáltató** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="55b36-188">Select **Azure Active Directory (Azure AD)** from hello **Add identity provider** dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon4.png)

16. <span data-ttu-id="55b36-190">Válassza ki az előfizetési szinten **alapvető**.</span><span class="sxs-lookup"><span data-stu-id="55b36-190">Select subscription level as **Basic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon5.png)

17. <span data-ttu-id="55b36-192">A hello **alkalmazás tulajdonságainak** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b36-192">On hello **App properties** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon6.png)

    <span data-ttu-id="55b36-194">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-194">a.</span></span> <span data-ttu-id="55b36-195">Másolás hello **App ID URI** értékét, és használja azt **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím** a hello **Kantega SSO FishEye/tégelyt tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="55b36-195">Copy hello **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on hello **Kantega SSO for FishEye/Crucible Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="55b36-196">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-196">b.</span></span> <span data-ttu-id="55b36-197">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-197">Click **Next**.</span></span>

18. <span data-ttu-id="55b36-198">A hello **metaadatok importálása** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b36-198">On hello **Metadata import** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon7.png)

    <span data-ttu-id="55b36-200">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-200">a.</span></span> <span data-ttu-id="55b36-201">Válassza ki **metaadat-fájlt a számítógépen lévő**, és az Azure-portálról letöltött feltöltés metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="55b36-201">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="55b36-202">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-202">b.</span></span> <span data-ttu-id="55b36-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-203">Click **Next**.</span></span>

19. <span data-ttu-id="55b36-204">A hello **nevét és az egyszeri bejelentkezés hely** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b36-204">On hello **Name and SSO location** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon8.png)

    <span data-ttu-id="55b36-206">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-206">a.</span></span> <span data-ttu-id="55b36-207">Adja hozzá az identitásszolgáltató hello nevét a **identitás szolgáltatónevet** szövegmező (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="55b36-207">Add Name of hello Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="55b36-208">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-208">b.</span></span> <span data-ttu-id="55b36-209">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-209">Click **Next**.</span></span>

20. <span data-ttu-id="55b36-210">Ellenőrizze a hello aláíró tanúsítvánnyal, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="55b36-210">Verify hello Signing certificate and click **Next**.</span></span>    

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon9.png)

21. <span data-ttu-id="55b36-212">A hello **halszemoptikát használt felhasználói fiókok** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b36-212">On hello **FishEye user accounts** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon10.png)

    <span data-ttu-id="55b36-214">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-214">a.</span></span> <span data-ttu-id="55b36-215">Válassza ki **felhasználók FishEye tartozó belső könyvtárban szükség esetén hozzon létre** , és írja be a hello megfelelő hello csoport felhasználói neve (lehet több nem.</span><span class="sxs-lookup"><span data-stu-id="55b36-215">Select **Create users in FishEye's internal Directory if needed** and enter hello appropriate name of hello group for users (can be multiple no.</span></span> <span data-ttu-id="55b36-216">vesszővel elválasztott csoportok).</span><span class="sxs-lookup"><span data-stu-id="55b36-216">of groups separated by comma).</span></span>

    <span data-ttu-id="55b36-217">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-217">b.</span></span> <span data-ttu-id="55b36-218">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-218">Click **Next**.</span></span>

22. <span data-ttu-id="55b36-219">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-219">Click **Finish**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon11.png)

23. <span data-ttu-id="55b36-221">A hello **tartományok ismert az Azure AD** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b36-221">On hello **Known domains for Azure AD** section, perform following steps:</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon12.png)

    <span data-ttu-id="55b36-223">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-223">a.</span></span> <span data-ttu-id="55b36-224">Válassza ki **tartományok ismert** hello lap hello bal oldali panelről.</span><span class="sxs-lookup"><span data-stu-id="55b36-224">Select **Known domains** from hello left panel of hello page.</span></span>

    <span data-ttu-id="55b36-225">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-225">b.</span></span> <span data-ttu-id="55b36-226">Adja meg hello **tartományok ismert** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="55b36-226">Enter domain name in hello **Known domains** textbox.</span></span>

    <span data-ttu-id="55b36-227">c.</span><span class="sxs-lookup"><span data-stu-id="55b36-227">c.</span></span> <span data-ttu-id="55b36-228">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-228">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="55b36-229">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="55b36-229">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="55b36-230">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="55b36-230">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="55b36-231">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="55b36-231">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="55b36-232">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55b36-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="55b36-233">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="55b36-233">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="55b36-235">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="55b36-235">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="55b36-236">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55b36-236">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="55b36-238">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="55b36-238">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="55b36-240">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55b36-240">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="55b36-242">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55b36-242">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="55b36-244">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-244">a.</span></span> <span data-ttu-id="55b36-245">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="55b36-245">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="55b36-246">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-246">b.</span></span> <span data-ttu-id="55b36-247">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="55b36-247">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="55b36-248">c.</span><span class="sxs-lookup"><span data-stu-id="55b36-248">c.</span></span> <span data-ttu-id="55b36-249">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="55b36-249">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="55b36-250">d.</span><span class="sxs-lookup"><span data-stu-id="55b36-250">d.</span></span> <span data-ttu-id="55b36-251">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-251">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a><span data-ttu-id="55b36-252">Egy Kantega SSO a FishEye/tégelyt tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55b36-252">Creating a Kantega SSO for FishEye/Crucible test user</span></span>

<span data-ttu-id="55b36-253">az Azure AD tooenable felhasználók toolog tooFishEye/tégelybe, akkor ki kell építenie FishEye/tégelyt be.</span><span class="sxs-lookup"><span data-stu-id="55b36-253">tooenable Azure AD users toolog in tooFishEye/Crucible, they must be provisioned into FishEye/Crucible.</span></span> <span data-ttu-id="55b36-254">A FishEye/tégelyt Kantega egyszeri Bejelentkezést egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="55b36-254">In Kantega SSO for FishEye/Crucible, provisioning is a manual task.</span></span>

<span data-ttu-id="55b36-255">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="55b36-255">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="55b36-256">Jelentkezzen be a helyi kiszolgálón tégelyt tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55b36-256">Log in tooyour Crucible on premise server as an administrator.</span></span>

2. <span data-ttu-id="55b36-257">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="55b36-257">Hover on cog and click hello **Users**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user1.png) 

3. <span data-ttu-id="55b36-259">A **felhasználók** szakasz lapra, majd **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="55b36-259">Under **Users** tab section, click **Add user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user2.png)

4. <span data-ttu-id="55b36-261">A hello **új felhasználó hozzáadása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55b36-261">On hello **Add New User** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user3.png) 

    <span data-ttu-id="55b36-263">a.</span><span class="sxs-lookup"><span data-stu-id="55b36-263">a.</span></span> <span data-ttu-id="55b36-264">A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="55b36-264">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="55b36-265">b.</span><span class="sxs-lookup"><span data-stu-id="55b36-265">b.</span></span> <span data-ttu-id="55b36-266">A hello **megjelenítendő név** szövegmező, például a Britta Simon hello felhasználó megjelenítendő nevét.</span><span class="sxs-lookup"><span data-stu-id="55b36-266">In hello **Display Name** textbox, type display name of hello user like Britta Simon.</span></span>
    
    <span data-ttu-id="55b36-267">c.</span><span class="sxs-lookup"><span data-stu-id="55b36-267">c.</span></span> <span data-ttu-id="55b36-268">A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="55b36-268">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="55b36-269">d.</span><span class="sxs-lookup"><span data-stu-id="55b36-269">d.</span></span> <span data-ttu-id="55b36-270">A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="55b36-270">In hello **Password** textbox, type hello password of user.</span></span>  

    <span data-ttu-id="55b36-271">e.</span><span class="sxs-lookup"><span data-stu-id="55b36-271">e.</span></span> <span data-ttu-id="55b36-272">A hello **jelszó megerősítése** szövegmezőhöz hello felhasználó jelszavát adja meg újból.</span><span class="sxs-lookup"><span data-stu-id="55b36-272">In hello **Confirm Password** textbox, reenter hello password of user.</span></span>

    <span data-ttu-id="55b36-273">f.</span><span class="sxs-lookup"><span data-stu-id="55b36-273">f.</span></span> <span data-ttu-id="55b36-274">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="55b36-274">Click **Add**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="55b36-275">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="55b36-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="55b36-276">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse FishEye/tégelyt az egyszeri Bejelentkezéses hozzáférést tooKantega megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="55b36-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKantega SSO for FishEye/Crucible.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="55b36-278">**tooassign Britta Simon tooKantega FishEye/tégelyt, az egyszeri bejelentkezés hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55b36-278">**tooassign Britta Simon tooKantega SSO for FishEye/Crucible, perform hello following steps:**</span></span>

1. <span data-ttu-id="55b36-279">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55b36-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="55b36-281">Hello alkalmazások listában válassza ki a **Kantega SSO FishEye/tégelyt a**.</span><span class="sxs-lookup"><span data-stu-id="55b36-281">In hello applications list, select **Kantega SSO for FishEye/Crucible**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

3. <span data-ttu-id="55b36-283">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="55b36-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="55b36-285">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="55b36-285">Click **Add** button.</span></span> <span data-ttu-id="55b36-286">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55b36-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="55b36-288">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="55b36-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="55b36-289">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55b36-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="55b36-290">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55b36-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="55b36-291">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="55b36-291">Testing single sign-on</span></span>

<span data-ttu-id="55b36-292">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="55b36-292">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="55b36-293">Hello Kantega SSO FishEye/tégelyt csempe a hozzáférési Panel hello kattintáskor FishEye/tégelyt alkalmazás kapja meg automatikusan bejelentkezett tooyour Kantega SSO.</span><span class="sxs-lookup"><span data-stu-id="55b36-293">When you click hello Kantega SSO for FishEye/Crucible tile in hello Access Panel, you should get automatically signed-on tooyour Kantega SSO for FishEye/Crucible application.</span></span>
<span data-ttu-id="55b36-294">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="55b36-294">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="55b36-295">További források</span><span class="sxs-lookup"><span data-stu-id="55b36-295">Additional resources</span></span>

* [<span data-ttu-id="55b36-296">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="55b36-296">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="55b36-297">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="55b36-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

