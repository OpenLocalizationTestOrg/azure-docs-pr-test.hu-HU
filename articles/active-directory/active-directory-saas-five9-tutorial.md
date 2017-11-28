---
title: "Oktatóanyag: Azure Active Directory-integráció adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a><span data-ttu-id="ee347-103">Oktatóanyag: Azure Active Directory-integráció adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel)</span><span class="sxs-lookup"><span data-stu-id="ee347-103">Tutorial: Azure Active Directory integration with Five9 Plus Adapter (CTI, Contact Center Agents)</span></span>

<span data-ttu-id="ee347-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ee347-104">In this tutorial, you learn how toointegrate Five9 Plus Adapter (CTI, Contact Center Agents) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ee347-105">Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ee347-105">Integrating Five9 Plus Adapter (CTI, Contact Center Agents) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ee347-106">Szabályozhatja az Azure AD hozzáférési tooFive9 rendelkező plusz Adapter (CTI, forduljon Center-ügynökökkel)</span><span class="sxs-lookup"><span data-stu-id="ee347-106">You can control in Azure AD who has access tooFive9 Plus Adapter (CTI, Contact Center Agents)</span></span>
- <span data-ttu-id="ee347-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFive9 plusz (CTI, forduljon Center-ügynökökkel) Adapter (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ee347-107">You can enable your users tooautomatically get signed-on tooFive9 Plus Adapter (CTI, Contact Center Agents) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ee347-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ee347-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ee347-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ee347-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee347-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ee347-110">Prerequisites</span></span>

<span data-ttu-id="ee347-111">tooconfigure az Azure AD-integrációs adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), a következő elemek hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="ee347-111">tooconfigure Azure AD integration with Five9 Plus Adapter (CTI, Contact Center Agents), you need hello following items:</span></span>

- <span data-ttu-id="ee347-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ee347-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ee347-113">Egy Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ee347-113">A Five9 Plus Adapter (CTI, Contact Center Agents) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ee347-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ee347-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ee347-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ee347-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ee347-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ee347-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ee347-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ee347-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ee347-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ee347-118">Scenario description</span></span>
<span data-ttu-id="ee347-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ee347-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ee347-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ee347-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ee347-121">Hello gyűjteményből Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee347-121">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
2. <span data-ttu-id="ee347-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ee347-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a><span data-ttu-id="ee347-123">Hello gyűjteményből Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee347-123">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
<span data-ttu-id="ee347-124">tooconfigure hello integrációs Five9 Plus adapter (CTI, forduljon Center-ügynökökkel), az Azure AD-be, szükség tooadd Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="ee347-124">tooconfigure hello integration of Five9 Plus Adapter (CTI, Contact Center Agents) into Azure AD, you need tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ee347-125">**tooadd Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ee347-125">**tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ee347-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ee347-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ee347-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ee347-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ee347-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ee347-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ee347-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ee347-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ee347-133">Hello keresési mezőbe, írja be a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**.</span><span class="sxs-lookup"><span data-stu-id="ee347-133">In hello search box, type **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. <span data-ttu-id="ee347-135">A hello eredmények panelen válassza a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ee347-135">In hello results panel, select **Five9 Plus Adapter (CTI, Contact Center Agents)**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ee347-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ee347-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ee347-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ee347-138">In this section, you configure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ee347-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel) tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ee347-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Five9 Plus Adapter (CTI, Contact Center Agents) is tooa user in Azure AD.</span></span> <span data-ttu-id="ee347-140">Ez azt jelenti hello kapcsolódó felhasználói Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="ee347-140">In other words, a link relationship between an Azure AD user and hello related user in Five9 Plus Adapter (CTI, Contact Center Agents) needs toobe established.</span></span>

<span data-ttu-id="ee347-141">Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel), rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ee347-141">In Five9 Plus Adapter (CTI, Contact Center Agents), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ee347-142">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), a következő építőelemeket toocomplete hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="ee347-142">tooconfigure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ee347-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ee347-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ee347-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ee347-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ee347-145">**[Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tesztfelhasználó létrehozása](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave egy megfelelője a Britta Simon Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel), amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ee347-145">**[Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - toohave a counterpart of Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ee347-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ee347-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ee347-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ee347-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ee347-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee347-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ee347-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ee347-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>

<span data-ttu-id="ee347-150">**tooconfigure az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ee347-150">**tooconfigure Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="ee347-151">Az Azure portál, a hello hello **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ee347-151">In hello Azure portal, on hello **Five9 Plus Adapter (CTI, Contact Center Agents)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ee347-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ee347-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. <span data-ttu-id="ee347-155">A hello **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ee347-155">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    <span data-ttu-id="ee347-157">a.</span><span class="sxs-lookup"><span data-stu-id="ee347-157">a.</span></span> <span data-ttu-id="ee347-158">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="ee347-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>

    |    <span data-ttu-id="ee347-159">Környezet</span><span class="sxs-lookup"><span data-stu-id="ee347-159">Environment</span></span>      |       <span data-ttu-id="ee347-160">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="ee347-160">URL</span></span>      |
    | :-- | :-- |
    | <span data-ttu-id="ee347-161">"Five9, valamint a Microsoft Dynamics CRM Adapter"</span><span class="sxs-lookup"><span data-stu-id="ee347-161">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | <span data-ttu-id="ee347-162">A "Five9 plusz Zendesk-adaptert"</span><span class="sxs-lookup"><span data-stu-id="ee347-162">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | <span data-ttu-id="ee347-163">"Five9, valamint az ügynök asztali eszközkészlet Adapter"</span><span class="sxs-lookup"><span data-stu-id="ee347-163">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    <span data-ttu-id="ee347-164">b.</span><span class="sxs-lookup"><span data-stu-id="ee347-164">b.</span></span> <span data-ttu-id="ee347-165">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="ee347-165">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    |      <span data-ttu-id="ee347-166">Környezet</span><span class="sxs-lookup"><span data-stu-id="ee347-166">Environment</span></span>     |      <span data-ttu-id="ee347-167">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="ee347-167">URL</span></span>      |
    | :--                  | :--           |
    | <span data-ttu-id="ee347-168">"Five9, valamint a Microsoft Dynamics CRM Adapter"</span><span class="sxs-lookup"><span data-stu-id="ee347-168">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | <span data-ttu-id="ee347-169">A "Five9 plusz Zendesk-adaptert"</span><span class="sxs-lookup"><span data-stu-id="ee347-169">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | <span data-ttu-id="ee347-170">"Five9, valamint az ügynök asztali eszközkészlet Adapter"</span><span class="sxs-lookup"><span data-stu-id="ee347-170">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. <span data-ttu-id="ee347-171">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ee347-171">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. <span data-ttu-id="ee347-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee347-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ee347-175">A hello **Five9 plusz (CTI, forduljon Center-ügynökökkel) adapterkonfiguráció** területén kattintson **konfigurálása Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** tooopen **bejelentkezéskonfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ee347-175">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** section, click **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ee347-176">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ee347-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. <span data-ttu-id="ee347-178">tooconfigure egyszeri bejelentkezést a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** oldalon kell letöltött toosend hello **Certificate(Base64), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[Five9 plusz (CTI, forduljon Center-ügynökökkel) támogatási adaptercsoport](https://www.five9.com/about/contact).</span><span class="sxs-lookup"><span data-stu-id="ee347-178">tooconfigure single sign-on on **Five9 Plus Adapter (CTI, Contact Center Agents)** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact).</span></span> <span data-ttu-id="ee347-179">Is emellett SSO további konfigurálásához kövesse hello következő lépések toohello adapter szerint:</span><span class="sxs-lookup"><span data-stu-id="ee347-179">Also additionally, for configuring SSO further please follow hello below steps according toohello adapter:</span></span>

    <span data-ttu-id="ee347-180">a.</span><span class="sxs-lookup"><span data-stu-id="ee347-180">a.</span></span> <span data-ttu-id="ee347-181">"Five9 Plus ügynök asztali eszközkészlet adaptert" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee347-181">“Five9 Plus Adapter for Agent Desktop Toolkit” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="ee347-182">b.</span><span class="sxs-lookup"><span data-stu-id="ee347-182">b.</span></span> <span data-ttu-id="ee347-183">"Five9 Plus Adapter a Microsoft Dynamics CRM" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee347-183">“Five9 Plus Adapter for Microsoft Dynamics CRM” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="ee347-184">c.</span><span class="sxs-lookup"><span data-stu-id="ee347-184">c.</span></span> <span data-ttu-id="ee347-185">"Five9 plusz Zendesk-adaptert" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee347-185">“Five9 Plus Adapter for Zendesk” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span></span>


> [!TIP]
> <span data-ttu-id="ee347-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ee347-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ee347-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ee347-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ee347-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ee347-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ee347-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee347-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="ee347-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ee347-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ee347-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ee347-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ee347-193">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ee347-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ee347-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ee347-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ee347-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee347-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ee347-199">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ee347-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ee347-201">a.</span><span class="sxs-lookup"><span data-stu-id="ee347-201">a.</span></span> <span data-ttu-id="ee347-202">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ee347-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ee347-203">b.</span><span class="sxs-lookup"><span data-stu-id="ee347-203">b.</span></span> <span data-ttu-id="ee347-204">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ee347-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ee347-205">c.</span><span class="sxs-lookup"><span data-stu-id="ee347-205">c.</span></span> <span data-ttu-id="ee347-206">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ee347-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ee347-207">d.</span><span class="sxs-lookup"><span data-stu-id="ee347-207">d.</span></span> <span data-ttu-id="ee347-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ee347-208">Click **Create**.</span></span>
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a><span data-ttu-id="ee347-209">Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee347-209">Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user</span></span>

<span data-ttu-id="ee347-210">Ebben a szakaszban egy felhasználó Britta Simon meghívta Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ee347-210">In this section, you create a user called Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents).</span></span> <span data-ttu-id="ee347-211">Együttműködve [Five9 plusz (CTI, forduljon Center-ügynökökkel) támogatási adaptercsoport](https://www.five9.com/about/contact) felhasználót is hozzáadhat hello hello Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) platform.</span><span class="sxs-lookup"><span data-stu-id="ee347-211">Work with [Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact) to add hello users in hello Five9 Plus Adapter (CTI, Contact Center Agents) platform.</span></span> <span data-ttu-id="ee347-212">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="ee347-212">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ee347-213">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ee347-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ee347-214">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFive9 Plus Adapter (CTI, forduljon Center-ügynökökkel) megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ee347-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFive9 Plus Adapter (CTI, Contact Center Agents).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ee347-216">**tooassign Britta Simon tooFive9 Plus Adapter (CTI, forduljon Center-ügynökökkel), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ee347-216">**tooassign Britta Simon tooFive9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="ee347-217">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ee347-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ee347-219">Hello alkalmazások listában válassza ki a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**.</span><span class="sxs-lookup"><span data-stu-id="ee347-219">In hello applications list, select **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. <span data-ttu-id="ee347-221">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ee347-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ee347-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee347-223">Click **Add** button.</span></span> <span data-ttu-id="ee347-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee347-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ee347-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ee347-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ee347-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee347-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ee347-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee347-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ee347-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ee347-229">Testing single sign-on</span></span>

<span data-ttu-id="ee347-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ee347-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ee347-231">Ha a hozzáférési Panel hello hello Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) csempe gombra kattint, automatikusan bejelentkezett tooyour Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="ee347-231">When you click hello Five9 Plus Adapter (CTI, Contact Center Agents) tile in hello Access Panel, you should get automatically signed-on tooyour Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>
<span data-ttu-id="ee347-232">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ee347-232">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee347-233">További források</span><span class="sxs-lookup"><span data-stu-id="ee347-233">Additional resources</span></span>

* [<span data-ttu-id="ee347-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ee347-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ee347-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ee347-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

