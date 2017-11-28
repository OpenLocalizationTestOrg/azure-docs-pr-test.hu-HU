---
title: "Oktatóanyag: Azure Active Directoryval integrált iLMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és iLMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="abbf9-103">Oktatóanyag: Azure Active Directoryval integrált iLMS</span><span class="sxs-lookup"><span data-stu-id="abbf9-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="abbf9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate iLMS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="abbf9-104">In this tutorial, you learn how toointegrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="abbf9-105">ILMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-105">Integrating iLMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="abbf9-106">Megadhatja a hozzáférés tooiLMS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="abbf9-106">You can control in Azure AD who has access tooiLMS</span></span>
- <span data-ttu-id="abbf9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooiLMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="abbf9-107">You can enable your users tooautomatically get signed-on tooiLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="abbf9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="abbf9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="abbf9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="abbf9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abbf9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="abbf9-110">Prerequisites</span></span>

<span data-ttu-id="abbf9-111">az Azure AD integrálása iLMS tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-111">tooconfigure Azure AD integration with iLMS, you need hello following items:</span></span>

- <span data-ttu-id="abbf9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="abbf9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="abbf9-113">Egy iLMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="abbf9-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="abbf9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="abbf9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="abbf9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="abbf9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="abbf9-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="abbf9-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="abbf9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abbf9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="abbf9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="abbf9-118">Scenario description</span></span>
<span data-ttu-id="abbf9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="abbf9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="abbf9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="abbf9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="abbf9-121">Hello gyűjteményből iLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="abbf9-121">Adding iLMS from hello gallery</span></span>
2. <span data-ttu-id="abbf9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="abbf9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-hello-gallery"></a><span data-ttu-id="abbf9-123">Hello gyűjteményből iLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="abbf9-123">Adding iLMS from hello gallery</span></span>
<span data-ttu-id="abbf9-124">tooconfigure hello integrációja iLMS az Azure AD-be, meg kell tooadd iLMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="abbf9-124">tooconfigure hello integration of iLMS into Azure AD, you need tooadd iLMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="abbf9-125">**tooadd iLMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="abbf9-125">**tooadd iLMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="abbf9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="abbf9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="abbf9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="abbf9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="abbf9-131">tooadd new application, click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="abbf9-133">Hello keresési mezőbe, írja be a **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-133">In hello search box, type **iLMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="abbf9-135">Hello eredmények panelen, jelölje ki a **iLMS**, majd kattintson a **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-135">In hello results panel, select **iLMS**, then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="abbf9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="abbf9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="abbf9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján iLMS.</span><span class="sxs-lookup"><span data-stu-id="abbf9-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="abbf9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó iLMS tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="abbf9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in iLMS is tooa user in Azure AD.</span></span> <span data-ttu-id="abbf9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello iLMS közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="abbf9-140">In other words, a link relationship between an Azure AD user and hello related user in iLMS needs toobe established.</span></span>

<span data-ttu-id="abbf9-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a iLMS.</span><span class="sxs-lookup"><span data-stu-id="abbf9-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in iLMS.</span></span>

<span data-ttu-id="abbf9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés iLMS-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="abbf9-142">tooconfigure and test Azure AD single sign-on with iLMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="abbf9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="abbf9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="abbf9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abbf9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="abbf9-145">**[Egy iLMS tesztfelhasználó létrehozása](#creating-an-ilms-test-user)**  -toohave Britta Simon iLMS, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="abbf9-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - toohave a counterpart of Britta Simon in iLMS that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="abbf9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="abbf9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="abbf9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="abbf9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="abbf9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="abbf9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="abbf9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az iLMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="abbf9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="abbf9-150">**az Azure AD tooconfigure egyszeri bejelentkezést a iLMS, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="abbf9-150">**tooconfigure Azure AD single sign-on with iLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="abbf9-151">Az Azure portál, a hello hello **iLMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-151">In hello Azure portal, on hello **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="abbf9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="abbf9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="abbf9-155">A hello **iLMS tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="abbf9-155">On hello **iLMS Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="abbf9-157">a.</span><span class="sxs-lookup"><span data-stu-id="abbf9-157">a.</span></span> <span data-ttu-id="abbf9-158">A hello **azonosítója** szövegmezőhöz Beillesztés hello **azonosítója** értéket másol a **szolgáltató** iLMS felügyeleti portál SAML-beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="abbf9-158">In hello **Identifier** textbox, paste hello **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="abbf9-159">b.</span><span class="sxs-lookup"><span data-stu-id="abbf9-159">b.</span></span> <span data-ttu-id="abbf9-160">A hello **válasz URL-CÍMEN** szövegmezőhöz Beillesztés hello **végpont URL-** értéket másol a **szolgáltató** szakasz hello következő rendelkező iLMS felügyeleti portál SAML-beállítások minta`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="abbf9-160">In hello **Reply URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having hello following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="abbf9-161">Ez 123456 példa érték azonosító.</span><span class="sxs-lookup"><span data-stu-id="abbf9-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="abbf9-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="abbf9-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="abbf9-164">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello **végpont URL-** értéket másol a **szolgáltató** szakasz iLMS felügyeleti portálon SAML-beállítások`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="abbf9-164">In hello **Sign-on URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="abbf9-165">a JIT-kiépítés, iLMS alkalmazás vár hello SAML helyességi feltételek egy meghatározott formátumban tooenable.</span><span class="sxs-lookup"><span data-stu-id="abbf9-165">tooenable JIT provisioning, iLMS application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="abbf9-166">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="abbf9-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="abbf9-167">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="abbf9-167">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="abbf9-168">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="abbf9-168">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="abbf9-170">Hozzon létre **részleg, régió** és **osztás** attribútumok közül, iLMS hello név attribútum hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="abbf9-170">Create **Department, Region** and **Division** attributes and add hello name of these attributes in iLMS.</span></span> <span data-ttu-id="abbf9-171">A fent látható összes attribútum megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="abbf9-171">All these attributes shown above are required.</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="abbf9-172">Tooenable rendelkezik **Un-recognized felhasználói fiók létrehozása** a iLMS toomap ezek az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="abbf9-172">You have tooenable **Create Un-recognized User Account** in iLMS toomap these attributes.</span></span> <span data-ttu-id="abbf9-173">Útmutatás alapján hello [Itt](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget hello attribútumok konfiguráció képet.</span><span class="sxs-lookup"><span data-stu-id="abbf9-173">Follow hello instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget an idea on hello attributes configuration.</span></span>

6. <span data-ttu-id="abbf9-174">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-174">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="abbf9-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="abbf9-175">Attribute Name</span></span> | <span data-ttu-id="abbf9-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="abbf9-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="abbf9-177">osztály</span><span class="sxs-lookup"><span data-stu-id="abbf9-177">division</span></span> | <span data-ttu-id="abbf9-178">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="abbf9-178">user.department</span></span> |
    | <span data-ttu-id="abbf9-179">Régió</span><span class="sxs-lookup"><span data-stu-id="abbf9-179">region</span></span> | <span data-ttu-id="abbf9-180">User.state</span><span class="sxs-lookup"><span data-stu-id="abbf9-180">user.state</span></span> |
    | <span data-ttu-id="abbf9-181">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="abbf9-181">department</span></span> | <span data-ttu-id="abbf9-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="abbf9-182">user.jobtitle</span></span> |

    <span data-ttu-id="abbf9-183">a.</span><span class="sxs-lookup"><span data-stu-id="abbf9-183">a.</span></span> <span data-ttu-id="abbf9-184">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abbf9-184">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="abbf9-187">b.</span><span class="sxs-lookup"><span data-stu-id="abbf9-187">b.</span></span> <span data-ttu-id="abbf9-188">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="abbf9-188">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="abbf9-189">c.</span><span class="sxs-lookup"><span data-stu-id="abbf9-189">c.</span></span> <span data-ttu-id="abbf9-190">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="abbf9-190">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="abbf9-191">d.</span><span class="sxs-lookup"><span data-stu-id="abbf9-191">d.</span></span> <span data-ttu-id="abbf9-192">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="abbf9-192">Click **Ok**</span></span>

7. <span data-ttu-id="abbf9-193">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="abbf9-193">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="abbf9-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-195">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="abbf9-197">Egy másik webes böngészőablakban, jelentkezzen be tooyour **iLMS felügyeleti portál** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="abbf9-197">In a different web browser window, log in tooyour **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="abbf9-198">Kattintson a **SSO:SAML** alatt **beállítások** tooopen SAML beállítások lapra, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-198">Click **SSO:SAML** under **Settings** tab tooopen SAML settings and perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="abbf9-200">a.</span><span class="sxs-lookup"><span data-stu-id="abbf9-200">a.</span></span> <span data-ttu-id="abbf9-201">Bontsa ki a hello **szolgáltató** szakaszt, és másolja hello **azonosító** és **végpont URL-** érték.</span><span class="sxs-lookup"><span data-stu-id="abbf9-201">Expand hello **Service Provider** section and copy hello **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="abbf9-203">b.</span><span class="sxs-lookup"><span data-stu-id="abbf9-203">b.</span></span> <span data-ttu-id="abbf9-204">A **identitásszolgáltató** kattintson **metaadatok importálása**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="abbf9-205">c.</span><span class="sxs-lookup"><span data-stu-id="abbf9-205">c.</span></span> <span data-ttu-id="abbf9-206">Jelölje be hello **metaadatok** az Azure-portálról letöltött fájl **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="abbf9-206">Select hello **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="abbf9-208">d.</span><span class="sxs-lookup"><span data-stu-id="abbf9-208">d.</span></span> <span data-ttu-id="abbf9-209">Ha azt szeretné, hogy tooenable JIT-kiépítés toocreate iLMS tartozó fiókok un-ismeri fel a felhasználók, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="abbf9-209">If you want tooenable JIT provisioning toocreate iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="abbf9-210">Ellenőrizze **nem felismerhető felhasználói fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="abbf9-212">Hello attribútumok megfeleltetése hello attribútumokkal iLMS az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="abbf9-212">Map hello attributes in Azure AD with hello attributes in iLMS.</span></span> <span data-ttu-id="abbf9-213">Hello attribútum oszlopában adja meg a hello attribútumok nevét vagy hello alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="abbf9-213">In hello attribute column, specify hello attributes name or hello default value.</span></span>

    <span data-ttu-id="abbf9-214">e.</span><span class="sxs-lookup"><span data-stu-id="abbf9-214">e.</span></span> <span data-ttu-id="abbf9-215">Nyissa meg túl**üzleti szabályok** lapra, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-215">Go too**Business Rules** tab and perform hello following steps:</span></span> 
        
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="abbf9-217">Ellenőrizze **Un-recognized régiók létrehozása, az osztályok és a szervezeti egységek** toocreate régiókban, osztályok és szervezeti egységek, már létező helyreállításkor hello egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="abbf9-217">Check **Create Un-recognized Regions, Divisions and Departments** toocreate Regions, Divisions, and Departments that do not already exist at hello time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="abbf9-218">Ellenőrizze **frissítés felhasználói profil során bejelentkezés** toospecify e hello profil frissül az egyes egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="abbf9-218">Check **Update User Profile During Sign-in** toospecify whether hello user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="abbf9-219">Ha hello **"Frissítés üres értékek a nem kötelező mezők a felhasználói profil"** beállítás be van jelölve, a nem kötelező profil mező üres lesz a bejelentkezés után is hello iLMS profil toocontain üres értékek tekinthetők.</span><span class="sxs-lookup"><span data-stu-id="abbf9-219">If hello **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause hello user’s iLMS profile toocontain blank values for those fields.</span></span>
        
       - <span data-ttu-id="abbf9-220">Ellenőrizze **hiba értesítő E-mail küldése** és tooreceive hello hiba értesítő e-mailt, ahová hello e-mail hello felhasználó adja meg.</span><span class="sxs-lookup"><span data-stu-id="abbf9-220">Check **Send Error Notification Email** and enter hello email of hello user where you want tooreceive hello error notification email.</span></span>

11. <span data-ttu-id="abbf9-221">Kattintson a **mentése** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-221">Click **Save** button toosave hello settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="abbf9-223">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="abbf9-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="abbf9-224">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="abbf9-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="abbf9-225">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="abbf9-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="abbf9-226">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="abbf9-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="abbf9-227">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="abbf9-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="abbf9-229">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="abbf9-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="abbf9-230">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="abbf9-232">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="abbf9-232">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="abbf9-234">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abbf9-234">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="abbf9-236">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="abbf9-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="abbf9-238">a.</span><span class="sxs-lookup"><span data-stu-id="abbf9-238">a.</span></span> <span data-ttu-id="abbf9-239">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="abbf9-240">b.</span><span class="sxs-lookup"><span data-stu-id="abbf9-240">b.</span></span> <span data-ttu-id="abbf9-241">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="abbf9-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="abbf9-242">c.</span><span class="sxs-lookup"><span data-stu-id="abbf9-242">c.</span></span> <span data-ttu-id="abbf9-243">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="abbf9-244">d.</span><span class="sxs-lookup"><span data-stu-id="abbf9-244">d.</span></span> <span data-ttu-id="abbf9-245">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="abbf9-246">Egy iLMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="abbf9-246">Creating an iLMS test user</span></span>

<span data-ttu-id="abbf9-247">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="abbf9-247">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="abbf9-248">Igény szerinti fog működni, ha akkor kattintott hello **Un-recognized felhasználói fiók létrehozása** jelölőnégyzet során iLMS felügyeleti portál a SAML-alapú konfigurációs beállítást.</span><span class="sxs-lookup"><span data-stu-id="abbf9-248">JIT will work, if you have clicked hello **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="abbf9-249">Ha egy felhasználó toocreate manuálisan kell, majd hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="abbf9-249">If you need toocreate an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="abbf9-250">Jelentkezzen be tooyour iLMS vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="abbf9-250">Log in tooyour iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="abbf9-251">Kattintson a **"Felhasználó regisztrálása"** alatt **felhasználók** tooopen lapon **regisztrálása felhasználói** lap.</span><span class="sxs-lookup"><span data-stu-id="abbf9-251">Click **“Register User”** under **Users** tab tooopen **Register User** page.</span></span> 
   
   ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="abbf9-253">A hello **"Felhasználó regisztrálása"** lapon, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="abbf9-253">On hello **“Register User”** page, perform hello following steps.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="abbf9-255">a.</span><span class="sxs-lookup"><span data-stu-id="abbf9-255">a.</span></span> <span data-ttu-id="abbf9-256">A hello **Keresztnév** szövegmezőhöz hello első típusnév Britta.</span><span class="sxs-lookup"><span data-stu-id="abbf9-256">In hello **First Name** textbox, type hello first name Britta.</span></span>
   
    <span data-ttu-id="abbf9-257">b.</span><span class="sxs-lookup"><span data-stu-id="abbf9-257">b.</span></span> <span data-ttu-id="abbf9-258">A hello **Vezetéknév** szövegmezőhöz típus hello Vezetéknév Simon.</span><span class="sxs-lookup"><span data-stu-id="abbf9-258">In hello **Last Name** textbox, type hello last name Simon.</span></span>

    <span data-ttu-id="abbf9-259">c.</span><span class="sxs-lookup"><span data-stu-id="abbf9-259">c.</span></span> <span data-ttu-id="abbf9-260">A hello **E-mail azonosító** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="abbf9-260">In hello **Email ID** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="abbf9-261">d.</span><span class="sxs-lookup"><span data-stu-id="abbf9-261">d.</span></span> <span data-ttu-id="abbf9-262">A hello **régió** legördülő menüben válassza hello érték régióhoz.</span><span class="sxs-lookup"><span data-stu-id="abbf9-262">In hello **Region** dropdown, select hello value for region.</span></span>

    <span data-ttu-id="abbf9-263">e.</span><span class="sxs-lookup"><span data-stu-id="abbf9-263">e.</span></span> <span data-ttu-id="abbf9-264">A hello **osztás** legördülő menüben válassza hello értéknek nullával.</span><span class="sxs-lookup"><span data-stu-id="abbf9-264">In hello **Division** dropdown, select hello value for division.</span></span>

    <span data-ttu-id="abbf9-265">f.</span><span class="sxs-lookup"><span data-stu-id="abbf9-265">f.</span></span> <span data-ttu-id="abbf9-266">A hello **részleg** legördülő menüben válassza hello érték részleg számára.</span><span class="sxs-lookup"><span data-stu-id="abbf9-266">In hello **Department** dropdown, select hello value for department.</span></span>

    <span data-ttu-id="abbf9-267">g.</span><span class="sxs-lookup"><span data-stu-id="abbf9-267">g.</span></span> <span data-ttu-id="abbf9-268">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="abbf9-269">Regisztrációs mail toouser kiválasztásával elküldheti **regisztrációs üzenet küldése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="abbf9-269">You can send registration mail toouser by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="abbf9-270">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="abbf9-270">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="abbf9-271">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooiLMS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="abbf9-271">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooiLMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="abbf9-273">**tooassign Britta Simon tooiLMS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="abbf9-273">**tooassign Britta Simon tooiLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="abbf9-274">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-274">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="abbf9-276">Hello alkalmazások listában válassza ki a **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-276">In hello applications list, select **iLMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="abbf9-278">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="abbf9-278">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="abbf9-280">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="abbf9-280">Click **Add** button.</span></span> <span data-ttu-id="abbf9-281">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abbf9-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="abbf9-283">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="abbf9-283">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="abbf9-284">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abbf9-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="abbf9-285">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abbf9-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="abbf9-286">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="abbf9-286">Testing single sign-on</span></span>

<span data-ttu-id="abbf9-287">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="abbf9-287">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="abbf9-288">Hello iLMS hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour iLMS alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="abbf9-288">When you click hello iLMS tile in hello Access Panel, you should get automatically signed-on tooyour iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abbf9-289">További források</span><span class="sxs-lookup"><span data-stu-id="abbf9-289">Additional resources</span></span>

* [<span data-ttu-id="abbf9-290">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="abbf9-290">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="abbf9-291">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="abbf9-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

