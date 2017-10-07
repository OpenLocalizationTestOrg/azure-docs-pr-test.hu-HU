---
title: "Oktatóanyag: Azure Active Directoryval integrált üzemeltetett grafit |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az üzemeltetett grafit között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d8914f6417ba8fbdef1a48e1b36635200ba130d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a><span data-ttu-id="28632-103">Oktatóanyag: Azure Active Directoryval integrált üzemeltetett grafit</span><span class="sxs-lookup"><span data-stu-id="28632-103">Tutorial: Azure Active Directory integration with Hosted Graphite</span></span>

<span data-ttu-id="28632-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate birtokolt grafit az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="28632-104">In this tutorial, you learn how toointegrate Hosted Graphite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="28632-105">Üzemeltetett grafit integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="28632-105">Integrating Hosted Graphite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="28632-106">Megadhatja a hozzáférés tooHosted grafit rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="28632-106">You can control in Azure AD who has access tooHosted Graphite</span></span>
- <span data-ttu-id="28632-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHosted grafit (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="28632-107">You can enable your users tooautomatically get signed-on tooHosted Graphite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="28632-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="28632-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="28632-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="28632-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28632-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28632-110">Prerequisites</span></span>

<span data-ttu-id="28632-111">tooconfigure üzemeltetett grafit az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="28632-111">tooconfigure Azure AD integration with Hosted Graphite, you need hello following items:</span></span>

- <span data-ttu-id="28632-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="28632-112">An Azure AD subscription</span></span>
- <span data-ttu-id="28632-113">Egy üzemeltetett grafit egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="28632-113">A Hosted Graphite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="28632-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="28632-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="28632-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="28632-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="28632-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="28632-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="28632-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28632-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="28632-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="28632-118">Scenario description</span></span>
<span data-ttu-id="28632-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="28632-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="28632-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="28632-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="28632-121">Hello gyűjteményből üzemeltetett grafit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28632-121">Adding Hosted Graphite from hello gallery</span></span>
2. <span data-ttu-id="28632-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="28632-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hosted-graphite-from-hello-gallery"></a><span data-ttu-id="28632-123">Hello gyűjteményből üzemeltetett grafit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28632-123">Adding Hosted Graphite from hello gallery</span></span>
<span data-ttu-id="28632-124">tooconfigure hello integrációs üzemeltetett grafit, az Azure AD-be, meg kell tooadd birtokolt grafit hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="28632-124">tooconfigure hello integration of Hosted Graphite into Azure AD, you need tooadd Hosted Graphite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="28632-125">**tooadd birtokolt grafit hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="28632-125">**tooadd Hosted Graphite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="28632-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="28632-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="28632-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="28632-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="28632-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28632-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="28632-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="28632-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="28632-133">Hello keresési mezőbe, írja be a **üzemeltetett grafit**.</span><span class="sxs-lookup"><span data-stu-id="28632-133">In hello search box, type **Hosted Graphite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. <span data-ttu-id="28632-135">A hello eredmények panelen válassza a **üzemeltetett grafit**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="28632-135">In hello results panel, select **Hosted Graphite**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="28632-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="28632-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="28632-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló szolgáltatott grafit.</span><span class="sxs-lookup"><span data-stu-id="28632-138">In this section, you configure and test Azure AD single sign-on with Hosted Graphite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="28632-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó üzemeltetett grafit tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="28632-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hosted Graphite is tooa user in Azure AD.</span></span> <span data-ttu-id="28632-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello üzemeltetett grafit közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="28632-140">In other words, a link relationship between an Azure AD user and hello related user in Hosted Graphite needs toobe established.</span></span>

<span data-ttu-id="28632-141">Az üzemeltetett grafit rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="28632-141">In Hosted Graphite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="28632-142">tooconfigure és az Azure AD az egyszeri bejelentkezés üzemeltetett grafit-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="28632-142">tooconfigure and test Azure AD single sign-on with Hosted Graphite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="28632-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="28632-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="28632-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="28632-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="28632-145">**[Üzemeltetett grafit tesztfelhasználó létrehozása](#creating-a-hosted-graphite-test-user)**  -toohave egy megfelelője a Britta Simon az üzemeltetett grafit, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="28632-145">**[Creating a Hosted Graphite test user](#creating-a-hosted-graphite-test-user)** - toohave a counterpart of Britta Simon in Hosted Graphite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="28632-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="28632-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="28632-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="28632-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="28632-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28632-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="28632-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az üzemeltetett grafit alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="28632-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hosted Graphite application.</span></span>

<span data-ttu-id="28632-150">**az Azure AD tooconfigure egyszeri bejelentkezést az üzemeltetett grafit hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="28632-150">**tooconfigure Azure AD single sign-on with Hosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="28632-151">Az Azure portál, a hello hello **üzemeltetett grafit** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28632-151">In hello Azure portal, on hello **Hosted Graphite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="28632-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="28632-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. <span data-ttu-id="28632-155">A hello **üzemeltetett grafit tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="28632-155">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    <span data-ttu-id="28632-157">a.</span><span class="sxs-lookup"><span data-stu-id="28632-157">a.</span></span> <span data-ttu-id="28632-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.hostedgraphite.com/metadata/<user id>`</span><span class="sxs-lookup"><span data-stu-id="28632-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/metadata/<user id>`</span></span>

    <span data-ttu-id="28632-159">b.</span><span class="sxs-lookup"><span data-stu-id="28632-159">b.</span></span> <span data-ttu-id="28632-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.hostedgraphite.com/complete/saml/<user id>`</span><span class="sxs-lookup"><span data-stu-id="28632-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/complete/saml/<user id>`</span></span>

4. <span data-ttu-id="28632-161">A hello **üzemeltetett grafit tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="28632-161">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    <span data-ttu-id="28632-163">a.</span><span class="sxs-lookup"><span data-stu-id="28632-163">a.</span></span> <span data-ttu-id="28632-164">Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="28632-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="28632-165">b.</span><span class="sxs-lookup"><span data-stu-id="28632-165">b.</span></span> <span data-ttu-id="28632-166">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.hostedgraphite.com/login/saml/<user id>/`</span><span class="sxs-lookup"><span data-stu-id="28632-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/login/saml/<user id>/`</span></span>   

    > [!NOTE] 
    > <span data-ttu-id="28632-167">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="28632-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="28632-168">Ezeket az értékeket a hello rendelkezik tooupdate tényleges azonosító, a válasz URL-CÍMEN és a bejelentkezési az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="28632-168">You have tooupdate these values with hello actual Identifier, Reply URL and Sign On URL.</span></span> <span data-ttu-id="28632-169">Ezek az értékek tooget, elvégezheti a tooAccess -> az alkalmazás oldalán, vagy forduljon a SAML-alapú telepítő [üzemeltetett grafit támogatási csoport](mailto:help@hostedgraphite.com).</span><span class="sxs-lookup"><span data-stu-id="28632-169">tooget these values, you can go tooAccess->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).</span></span>
    >
 
5. <span data-ttu-id="28632-170">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="28632-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. <span data-ttu-id="28632-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="28632-172">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="28632-174">A hello **üzemeltetett grafit konfigurációs** kattintson **üzemeltetett grafit konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="28632-174">On hello **Hosted Graphite Configuration** section, click **Configure Hosted Graphite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="28632-175">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="28632-175">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. <span data-ttu-id="28632-177">Bejelentkezés tooyour üzemeltetett grafit Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="28632-177">Sign-on tooyour Hosted Graphite tenant as an administrator.</span></span>

9. <span data-ttu-id="28632-178">Nyissa meg toohello **SAML-alapú telepítő oldalán** hello oldalsávon (**Access -> SAML-alapú telepítő**).</span><span class="sxs-lookup"><span data-stu-id="28632-178">Go toohello **SAML Setup page** in hello sidebar (**Access -> SAML Setup**).</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. <span data-ttu-id="28632-180">Az URL-címek egyezniük a konfigurációt a hello elvégzett **üzemeltetett grafit tartomány és az URL-címek** hello Azure-portálon szakasza.</span><span class="sxs-lookup"><span data-stu-id="28632-180">Confirm these URls match your configuration done on hello **Hosted Graphite Domain and URLs** section of hello Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. <span data-ttu-id="28632-182">A **entitás vagy a kibocsátó azonosító** és **Egyszeri bejelentkezési URL-cím** szövegmezőből, illessze be a hello értékének **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="28632-182">In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste hello value of **SAML Entity ID** and **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. <span data-ttu-id="28632-184">Jelölje ki "**írásvédett**" mint **felhasználói szerepkör alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="28632-184">Select "**Read-only**" as **Default User Role**.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. <span data-ttu-id="28632-186">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="28632-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. <span data-ttu-id="28632-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="28632-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="28632-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="28632-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="28632-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="28632-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="28632-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="28632-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="28632-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="28632-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="28632-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="28632-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="28632-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="28632-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="28632-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="28632-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="28632-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="28632-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="28632-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28632-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="28632-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="28632-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="28632-204">a.</span><span class="sxs-lookup"><span data-stu-id="28632-204">a.</span></span> <span data-ttu-id="28632-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="28632-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="28632-206">b.</span><span class="sxs-lookup"><span data-stu-id="28632-206">b.</span></span> <span data-ttu-id="28632-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="28632-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="28632-208">c.</span><span class="sxs-lookup"><span data-stu-id="28632-208">c.</span></span> <span data-ttu-id="28632-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="28632-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="28632-210">d.</span><span class="sxs-lookup"><span data-stu-id="28632-210">d.</span></span> <span data-ttu-id="28632-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28632-211">Click **Create**.</span></span>
 
### <a name="creating-a-hosted-graphite-test-user"></a><span data-ttu-id="28632-212">Üzemeltetett grafit tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="28632-212">Creating a Hosted Graphite test user</span></span>

<span data-ttu-id="28632-213">hello ebben a szakaszban célja toocreate üzemeltetett grafit Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="28632-213">hello objective of this section is toocreate a user called Britta Simon in Hosted Graphite.</span></span> <span data-ttu-id="28632-214">Üzemeltetett grafit támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="28632-214">Hosted Graphite supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="28632-215">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="28632-215">There is no action item for you in this section.</span></span> <span data-ttu-id="28632-216">Új felhasználó létrejön egy kísérlet tooaccess birtokolt grafit során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="28632-216">A new user will be created during an attempt tooaccess Hosted Graphite if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="28632-217">Manuálisan kell toocreate egy felhasználót, ha szüksége van-e toocontact hello üzemeltetett grafit támogatási csoport keresztül < mailto:help@hostedgraphite.com >.</span><span class="sxs-lookup"><span data-stu-id="28632-217">If you need toocreate a user manually, you need toocontact hello Hosted Graphite support team via <mailto:help@hostedgraphite.com>.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="28632-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="28632-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="28632-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHosted grafit megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="28632-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHosted Graphite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="28632-221">**tooassign Britta Simon tooHosted grafit, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="28632-221">**tooassign Britta Simon tooHosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="28632-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28632-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="28632-224">Hello alkalmazások listában válassza ki a **üzemeltetett grafit**.</span><span class="sxs-lookup"><span data-stu-id="28632-224">In hello applications list, select **Hosted Graphite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. <span data-ttu-id="28632-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="28632-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="28632-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="28632-228">Click **Add** button.</span></span> <span data-ttu-id="28632-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28632-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="28632-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="28632-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="28632-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28632-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="28632-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28632-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="28632-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="28632-234">Testing single sign-on</span></span>

<span data-ttu-id="28632-235">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="28632-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="28632-236">Hello üzemeltetett grafit csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour grafit üzemeltetett alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="28632-236">When you click hello Hosted Graphite tile in hello Access Panel, you should get automatically signed-on tooyour Hosted Graphite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28632-237">További források</span><span class="sxs-lookup"><span data-stu-id="28632-237">Additional resources</span></span>

* [<span data-ttu-id="28632-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="28632-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="28632-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="28632-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

