---
title: "Oktatóanyag: Azure Active Directory-integráció EthicsPoint Incident Management (EPIM) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és EthicsPoint Incident Management (EPIM) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 73ef5fab815cddb3728f4b23173f99e62aec5bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a><span data-ttu-id="c384f-103">Oktatóanyag: Azure Active Directory-integráció EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="c384f-103">Tutorial: Azure Active Directory integration with EthicsPoint Incident Management (EPIM)</span></span>

<span data-ttu-id="c384f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate EthicsPoint Incident Management (EPIM) az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c384f-104">In this tutorial, you learn how toointegrate EthicsPoint Incident Management (EPIM) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c384f-105">EthicsPoint Incident Management (EPIM) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c384f-105">Integrating EthicsPoint Incident Management (EPIM) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c384f-106">Megadhatja a hozzáférés tooEthicsPoint Incident Management (EPIM) rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c384f-106">You can control in Azure AD who has access tooEthicsPoint Incident Management (EPIM)</span></span>
- <span data-ttu-id="c384f-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEthicsPoint Incident Management (EPIM) (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="c384f-107">You can enable your users tooautomatically get signed-on tooEthicsPoint Incident Management (EPIM) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c384f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c384f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c384f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c384f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c384f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c384f-110">Prerequisites</span></span>

<span data-ttu-id="c384f-111">tooconfigure az Azure AD-integrációs EthicsPoint Incident Management (EPIM), a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="c384f-111">tooconfigure Azure AD integration with EthicsPoint Incident Management (EPIM), you need hello following items:</span></span>

- <span data-ttu-id="c384f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c384f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c384f-113">Egy EthicsPoint Incident Management (EPIM) egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c384f-113">A EthicsPoint Incident Management (EPIM) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c384f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c384f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c384f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c384f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c384f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c384f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c384f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c384f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c384f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c384f-118">Scenario description</span></span>
<span data-ttu-id="c384f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c384f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c384f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c384f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c384f-121">Hello gyűjteményből EthicsPoint Incident Management (EPIM) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c384f-121">Adding EthicsPoint Incident Management (EPIM) from hello gallery</span></span>
2. <span data-ttu-id="c384f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c384f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ethicspoint-incident-management-epim-from-hello-gallery"></a><span data-ttu-id="c384f-123">Hello gyűjteményből EthicsPoint Incident Management (EPIM) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c384f-123">Adding EthicsPoint Incident Management (EPIM) from hello gallery</span></span>
<span data-ttu-id="c384f-124">tooconfigure hello integrációs EthicsPoint Incident Management (EPIM) az Azure AD-be, meg kell tooadd EthicsPoint Incident Management (EPIM) hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c384f-124">tooconfigure hello integration of EthicsPoint Incident Management (EPIM) into Azure AD, you need tooadd EthicsPoint Incident Management (EPIM) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c384f-125">**tooadd EthicsPoint Incident Management (EPIM) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c384f-125">**tooadd EthicsPoint Incident Management (EPIM) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c384f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c384f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c384f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c384f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c384f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c384f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c384f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c384f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c384f-133">Hello keresési mezőbe, írja be a **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="c384f-133">In hello search box, type **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. <span data-ttu-id="c384f-135">A hello eredmények panelen válassza ki a **EthicsPoint Incident Management (EPIM)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c384f-135">In hello results panel, select **EthicsPoint Incident Management (EPIM)**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c384f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c384f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c384f-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a EthicsPoint Incident Management (EPIM) "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="c384f-138">In this section, you configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM) based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c384f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói EthicsPoint Incident Management (EPIM) tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c384f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EthicsPoint Incident Management (EPIM) is tooa user in Azure AD.</span></span> <span data-ttu-id="c384f-140">Ez azt jelenti hello kapcsolódó felhasználói EthicsPoint Incident Management (EPIM) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c384f-140">In other words, a link relationship between an Azure AD user and hello related user in EthicsPoint Incident Management (EPIM) needs toobe established.</span></span>

<span data-ttu-id="c384f-141">EthicsPoint Incident Management (EPIM) rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c384f-141">In EthicsPoint Incident Management (EPIM), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c384f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése EthicsPoint Incident Management (EPIM), a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c384f-142">tooconfigure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c384f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c384f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c384f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c384f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c384f-145">**[EthicsPoint Incident Management (EPIM) tesztfelhasználó létrehozása](#creating-a-ethicspoint-incident-management-epim-test-user)**  -toohave egy megfelelője a Britta Simon a EthicsPoint incidens Management (EPIM), amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c384f-145">**[Creating a EthicsPoint Incident Management (EPIM) test user](#creating-a-ethicspoint-incident-management-epim-test-user)** - toohave a counterpart of Britta Simon in EthicsPoint Incident Management (EPIM) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c384f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c384f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c384f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c384f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c384f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c384f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c384f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az EthicsPoint Incident Management (EPIM) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c384f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EthicsPoint Incident Management (EPIM) application.</span></span>

<span data-ttu-id="c384f-150">**tooconfigure az Azure AD egyszeri bejelentkezést a EthicsPoint Incident Management (EPIM), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c384f-150">**tooconfigure Azure AD single sign-on with EthicsPoint Incident Management (EPIM), perform hello following steps:**</span></span>

1. <span data-ttu-id="c384f-151">Az Azure portál, a hello hello **EthicsPoint Incident Management (EPIM)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c384f-151">In hello Azure portal, on hello **EthicsPoint Incident Management (EPIM)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c384f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c384f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. <span data-ttu-id="c384f-155">A hello **EthicsPoint Incident Management (EPIM) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c384f-155">On hello **EthicsPoint Incident Management (EPIM) Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    <span data-ttu-id="c384f-157">a.</span><span class="sxs-lookup"><span data-stu-id="c384f-157">a.</span></span> <span data-ttu-id="c384f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="c384f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    <span data-ttu-id="c384f-159">b.</span><span class="sxs-lookup"><span data-stu-id="c384f-159">b.</span></span> <span data-ttu-id="c384f-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.navexglobal.com/adfs/services/trust`</span><span class="sxs-lookup"><span data-stu-id="c384f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.navexglobal.com/adfs/services/trust`</span></span>

    <span data-ttu-id="c384f-161">c.</span><span class="sxs-lookup"><span data-stu-id="c384f-161">c.</span></span> <span data-ttu-id="c384f-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.navexglobal.com/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="c384f-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<servername>.navexglobal.com/adfs/ls/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c384f-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c384f-163">These values are not real.</span></span> <span data-ttu-id="c384f-164">Frissítheti ezeket az értékeket hello tényleges válasz URL-CÍMEN, a azonosítója és a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="c384f-164">Update these values with hello actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="c384f-165">Ügyfél [EthicsPoint Incident Management (EPIM) ügyfél-támogatási csoport](http://www.navexglobal.com/company/contact-us) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c384f-165">Contact [EthicsPoint Incident Management (EPIM) Client support team](http://www.navexglobal.com/company/contact-us) tooget these values.</span></span> 

4. <span data-ttu-id="c384f-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c384f-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. <span data-ttu-id="c384f-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c384f-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="c384f-170">tooconfigure egyszeri bejelentkezést a **EthicsPoint Incident Management (EPIM)** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[EthicsPoint Incident Management (EPIM) támogatása Team](http://www.navexglobal.com/company/contact-us).</span><span class="sxs-lookup"><span data-stu-id="c384f-170">tooconfigure single sign-on on **EthicsPoint Incident Management (EPIM)** side, you need toosend hello downloaded **Metadata XML** too[EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="c384f-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c384f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c384f-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c384f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c384f-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c384f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c384f-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c384f-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="c384f-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c384f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c384f-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c384f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c384f-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c384f-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c384f-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c384f-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c384f-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c384f-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c384f-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c384f-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c384f-186">a.</span><span class="sxs-lookup"><span data-stu-id="c384f-186">a.</span></span> <span data-ttu-id="c384f-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c384f-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c384f-188">b.</span><span class="sxs-lookup"><span data-stu-id="c384f-188">b.</span></span> <span data-ttu-id="c384f-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c384f-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c384f-190">c.</span><span class="sxs-lookup"><span data-stu-id="c384f-190">c.</span></span> <span data-ttu-id="c384f-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c384f-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c384f-192">d.</span><span class="sxs-lookup"><span data-stu-id="c384f-192">d.</span></span> <span data-ttu-id="c384f-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c384f-193">Click **Create**.</span></span>
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a><span data-ttu-id="c384f-194">EthicsPoint Incident Management (EPIM) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c384f-194">Creating a EthicsPoint Incident Management (EPIM) test user</span></span>

<span data-ttu-id="c384f-195">Ebben a szakaszban egy Britta Simon EthicsPoint Incident Management (EPIM) nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c384f-195">In this section, you create a user called Britta Simon in EthicsPoint Incident Management (EPIM).</span></span> <span data-ttu-id="c384f-196">Adjon együttműködve [EthicsPoint Incident Management (EPIM) támogatási csoport](http://www.navexglobal.com/company/contact-us) tooadd hello felhasználók hello EthicsPoint Incident Management (EPIM) platform.</span><span class="sxs-lookup"><span data-stu-id="c384f-196">Please work with [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us) tooadd hello users in hello EthicsPoint Incident Management (EPIM) platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c384f-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c384f-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c384f-198">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooEthicsPoint Incident Management (EPIM) megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="c384f-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEthicsPoint Incident Management (EPIM).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c384f-200">**tooassign Britta Simon tooEthicsPoint Incident Management (EPIM), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c384f-200">**tooassign Britta Simon tooEthicsPoint Incident Management (EPIM), perform hello following steps:**</span></span>

1. <span data-ttu-id="c384f-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c384f-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c384f-203">Hello alkalmazások listában válassza ki a **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="c384f-203">In hello applications list, select **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. <span data-ttu-id="c384f-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c384f-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c384f-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c384f-207">Click **Add** button.</span></span> <span data-ttu-id="c384f-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c384f-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c384f-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c384f-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c384f-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c384f-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c384f-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c384f-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c384f-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c384f-213">Testing single sign-on</span></span>

<span data-ttu-id="c384f-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c384f-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="c384f-215">A hozzáférési Panel hello hello EthicsPoint Incident Management (EPIM) csempére kattintva kapja meg automatikusan bejelentkezett tooyour EthicsPoint Incident Management (EPIM) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c384f-215">When you click hello EthicsPoint Incident Management (EPIM) tile in hello Access Panel, you should get automatically signed-on tooyour EthicsPoint Incident Management (EPIM) application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c384f-216">További források</span><span class="sxs-lookup"><span data-stu-id="c384f-216">Additional resources</span></span>

* [<span data-ttu-id="c384f-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c384f-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c384f-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c384f-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

