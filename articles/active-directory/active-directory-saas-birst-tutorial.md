---
title: "Oktatóanyag: Azure Active Directoryval integrált Birst megalapozott üzleti Analytics |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Birst gyors üzleti elemzések között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: f007edcec0fb8ece215ab69f7ec7ca59ca34bddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a><span data-ttu-id="01718-103">Oktatóanyag: Azure Active Directoryval integrált Birst megalapozott üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-103">Tutorial: Azure Active Directory integration with Birst Agile Business Analytics</span></span>

<span data-ttu-id="01718-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Birst megalapozott üzleti Analytics az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01718-104">In this tutorial, you learn how toointegrate Birst Agile Business Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01718-105">Birst megalapozott üzleti Analytics integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="01718-105">Integrating Birst Agile Business Analytics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="01718-106">Szabályozhatja az Azure AD, aki rendelkezik hozzáférési tooBirst megalapozott üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-106">You can control in Azure AD who has access tooBirst Agile Business Analytics</span></span>
- <span data-ttu-id="01718-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBirst (egyszeri bejelentkezés) a nagy üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-107">You can enable your users tooautomatically get signed-on tooBirst Agile Business Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01718-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="01718-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="01718-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01718-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01718-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="01718-110">Prerequisites</span></span>

<span data-ttu-id="01718-111">tooconfigure Birst megalapozott üzleti Analytics az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="01718-111">tooconfigure Azure AD integration with Birst Agile Business Analytics, you need hello following items:</span></span>

- <span data-ttu-id="01718-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="01718-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01718-113">Egy Birst megalapozott üzleti Analytics egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="01718-113">A Birst Agile Business Analytics single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01718-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="01718-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01718-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="01718-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01718-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="01718-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01718-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01718-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01718-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="01718-118">Scenario description</span></span>
<span data-ttu-id="01718-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="01718-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01718-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="01718-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01718-121">Hello gyűjteményből hozzáadása a Birst megalapozott üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-121">Adding Birst Agile Business Analytics from hello gallery</span></span>
2. <span data-ttu-id="01718-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01718-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-birst-agile-business-analytics-from-hello-gallery"></a><span data-ttu-id="01718-123">Hello gyűjteményből hozzáadása a Birst megalapozott üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-123">Adding Birst Agile Business Analytics from hello gallery</span></span>
<span data-ttu-id="01718-124">tooconfigure hello integrációja Birst megalapozott üzleti Analytics az Azure AD-be, meg kell tooadd Birst megalapozott üzleti Analytics hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="01718-124">tooconfigure hello integration of Birst Agile Business Analytics into Azure AD, you need tooadd Birst Agile Business Analytics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="01718-125">**tooadd Birst megalapozott üzleti Analytics hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01718-125">**tooadd Birst Agile Business Analytics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="01718-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01718-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01718-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="01718-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="01718-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01718-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="01718-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="01718-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="01718-133">Hello keresési mezőbe, írja be a **Birst megalapozott üzleti Analytics**.</span><span class="sxs-lookup"><span data-stu-id="01718-133">In hello search box, type **Birst Agile Business Analytics**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/tutorial_birst_search.png)

5. <span data-ttu-id="01718-135">A hello eredmények panelen válassza a **Birst megalapozott üzleti elemzés**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="01718-135">In hello results panel, select **Birst Agile Business Analytics**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01718-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01718-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01718-138">Ebben a szakaszban konfigurálhatja, és tesztelés az Azure AD az egyszeri bejelentkezés Birst "Britta Simon." nevű tesztfelhasználó alapján a nagy üzleti elemzés</span><span class="sxs-lookup"><span data-stu-id="01718-138">In this section, you configure and test Azure AD single sign-on with Birst Agile Business Analytics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="01718-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Birst megalapozott üzleti Analytics tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="01718-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Birst Agile Business Analytics is tooa user in Azure AD.</span></span> <span data-ttu-id="01718-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Birst megalapozott üzleti Analytics közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="01718-140">In other words, a link relationship between an Azure AD user and hello related user in Birst Agile Business Analytics needs toobe established.</span></span>

<span data-ttu-id="01718-141">A nagy üzleti Analytics Birst, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="01718-141">In Birst Agile Business Analytics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="01718-142">tooconfigure és a vizsgálat az Azure AD egyszeri bejelentkezést a Birst megalapozott üzleti elemzés a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="01718-142">tooconfigure and test Azure AD single sign-on with Birst Agile Business Analytics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="01718-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="01718-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="01718-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01718-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01718-145">**[Birst megalapozott üzleti Analytics tesztfelhasználó létrehozása](#creating-a-birst-agile-business-analytics-test-user)**  -toohave egy megfelelője a Britta Simon Birst megalapozott üzleti Analytics, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="01718-145">**[Creating a Birst Agile Business Analytics test user](#creating-a-birst-agile-business-analytics-test-user)** - toohave a counterpart of Britta Simon in Birst Agile Business Analytics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="01718-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01718-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01718-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="01718-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01718-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01718-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01718-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Birst megalapozott üzleti Analytics alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="01718-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Birst Agile Business Analytics application.</span></span>

<span data-ttu-id="01718-150">**tooconfigure az Azure AD egyszeri bejelentkezést a nagy üzleti Analytics Birst, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01718-150">**tooconfigure Azure AD single sign-on with Birst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="01718-151">Az Azure portál, a hello hello **Birst megalapozott üzleti elemzés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="01718-151">In hello Azure portal, on hello **Birst Agile Business Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="01718-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01718-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_birst_samlbase.png)

3. <span data-ttu-id="01718-155">A hello **Birst megalapozott üzleti Analytics tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01718-155">On hello **Birst Agile Business Analytics Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_birst_url.png)

     <span data-ttu-id="01718-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="01718-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

     <span data-ttu-id="01718-158">hello URL-CÍMÉT, hogy a Birst fiók tartozik hello datacenter függ:</span><span class="sxs-lookup"><span data-stu-id="01718-158">hello URL depends on hello datacenter that your Birst account is located:</span></span> 

     * <span data-ttu-id="01718-159">Az Egyesült Államok datacenter használja a következő hello mintát:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="01718-159">For US datacenter use following hello pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span> 

     * <span data-ttu-id="01718-160">Európa datacenter használja a következő mintát hello:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="01718-160">For Europe datacenter use hello following pattern: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01718-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="01718-161">This value is not real.</span></span> <span data-ttu-id="01718-162">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="01718-162">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="01718-163">Ügyfél [Birst megalapozott üzleti Analytics ügyfél-támogatási csoport](mailto:info@birst.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="01718-163">Contact [Birst Agile Business Analytics Client support team](mailto:info@birst.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="01718-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="01718-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_birst_certificate.png) 

5. <span data-ttu-id="01718-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="01718-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="01718-168">A hello **Birst megalapozott üzleti konfigurációja** kattintson **konfigurálása Birst megalapozott üzleti elemzés** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="01718-168">On hello **Birst Agile Business Analytics Configuration** section, click **Configure Birst Agile Business Analytics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="01718-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="01718-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_birst_configure.png) 

7. <span data-ttu-id="01718-171">tooconfigure egyszeri bejelentkezést a **Birst megalapozott üzleti elemzés** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezést. URL-címe** túl[Birst megalapozott üzleti Analytics támogatási csoport](mailto:info@birst.com).</span><span class="sxs-lookup"><span data-stu-id="01718-171">tooconfigure single sign-on on **Birst Agile Business Analytics** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Birst Agile Business Analytics support team](mailto:info@birst.com).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="01718-172">Említse meg, hogy ez az integráció kell-e az SHA-256 algoritmus tooBirst team (SHA1 nem támogatott), például, hogy azok beállíthatja az hello SSO hello megfelelő kiszolgálói **app2101** stb.</span><span class="sxs-lookup"><span data-stu-id="01718-172">Mention tooBirst team that this integration needs SHA256 Algorithm (SHA1 will not be supported) so that they can set hello SSO on hello appropriate server like **app2101** etc.</span></span>
  

> [!TIP]
> <span data-ttu-id="01718-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="01718-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="01718-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="01718-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="01718-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01718-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01718-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01718-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="01718-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="01718-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="01718-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="01718-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="01718-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01718-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01718-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="01718-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01718-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01718-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01718-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01718-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01718-188">a.</span><span class="sxs-lookup"><span data-stu-id="01718-188">a.</span></span> <span data-ttu-id="01718-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01718-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01718-190">b.</span><span class="sxs-lookup"><span data-stu-id="01718-190">b.</span></span> <span data-ttu-id="01718-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01718-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01718-192">c.</span><span class="sxs-lookup"><span data-stu-id="01718-192">c.</span></span> <span data-ttu-id="01718-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="01718-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="01718-194">d.</span><span class="sxs-lookup"><span data-stu-id="01718-194">d.</span></span> <span data-ttu-id="01718-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="01718-195">Click **Create**.</span></span>
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a><span data-ttu-id="01718-196">Birst megalapozott üzleti Analytics tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01718-196">Creating a Birst Agile Business Analytics test user</span></span>

<span data-ttu-id="01718-197">hello ebben a szakaszban célja toocreate Britta Simon Birst megalapozott üzleti Analytics nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="01718-197">hello objective of this section is toocreate a user called Britta Simon in Birst Agile Business Analytics.</span></span> <span data-ttu-id="01718-198">Együttműködve [Birst megalapozott üzleti Analytics támogatási csoport](mailto:info@birst.com) tooadd hello felhasználók hello Birst fiók.</span><span class="sxs-lookup"><span data-stu-id="01718-198">Work with [Birst Agile Business Analytics support team](mailto:info@birst.com) tooadd hello users in hello Birst account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="01718-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="01718-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="01718-200">Ebben a szakaszban Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBirst megalapozott üzleti Analytics megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="01718-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBirst Agile Business Analytics.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="01718-202">**tooassign Britta Simon tooBirst üzleti gyors elemzés, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01718-202">**tooassign Britta Simon tooBirst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="01718-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01718-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="01718-205">Hello alkalmazások listában válassza ki a **Birst megalapozott üzleti Analytics**.</span><span class="sxs-lookup"><span data-stu-id="01718-205">In hello applications list, select **Birst Agile Business Analytics**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-birst-tutorial/tutorial_birst_app.png) 

3. <span data-ttu-id="01718-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="01718-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="01718-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="01718-209">Click **Add** button.</span></span> <span data-ttu-id="01718-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01718-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="01718-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="01718-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="01718-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01718-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01718-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01718-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01718-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="01718-215">Testing single sign-on</span></span>

<span data-ttu-id="01718-216">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="01718-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="01718-217">Hello Birst megalapozott üzleti Analytics hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Birst megalapozott üzleti Analytics alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="01718-217">When you click hello Birst Agile Business Analytics tile in hello Access Panel, you should get automatically signed-on tooyour Birst Agile Business Analytics application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="01718-218">További források</span><span class="sxs-lookup"><span data-stu-id="01718-218">Additional resources</span></span>

* [<span data-ttu-id="01718-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="01718-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01718-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="01718-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-birst-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png

