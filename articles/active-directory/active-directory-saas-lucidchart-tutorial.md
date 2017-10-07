---
title: "Oktatóanyag: Azure Active Directoryval integrált Lucidchart |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Lucidchart között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 774e5f423097650a3cae8e8ca13b2c65b8470736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="3aaee-103">Oktatóanyag: Azure Active Directoryval integrált Lucidchart</span><span class="sxs-lookup"><span data-stu-id="3aaee-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="3aaee-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lucidchart az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3aaee-104">In this tutorial, you learn how toointegrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3aaee-105">Lucidchart integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="3aaee-105">Integrating Lucidchart with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3aaee-106">Megadhatja a hozzáférés tooLucidchart rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3aaee-106">You can control in Azure AD who has access tooLucidchart</span></span>
- <span data-ttu-id="3aaee-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLucidchart (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3aaee-107">You can enable your users tooautomatically get signed-on tooLucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3aaee-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3aaee-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3aaee-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3aaee-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3aaee-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3aaee-110">Prerequisites</span></span>

<span data-ttu-id="3aaee-111">az Azure AD integrálása Lucidchart tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="3aaee-111">tooconfigure Azure AD integration with Lucidchart, you need hello following items:</span></span>

- <span data-ttu-id="3aaee-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3aaee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3aaee-113">Egy Lucidchart egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3aaee-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3aaee-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3aaee-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3aaee-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="3aaee-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3aaee-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3aaee-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3aaee-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3aaee-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3aaee-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3aaee-118">Scenario description</span></span>
<span data-ttu-id="3aaee-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3aaee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3aaee-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3aaee-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3aaee-121">Hello gyűjteményből Lucidchart hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3aaee-121">Adding Lucidchart from hello gallery</span></span>
2. <span data-ttu-id="3aaee-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3aaee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-hello-gallery"></a><span data-ttu-id="3aaee-123">Hello gyűjteményből Lucidchart hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3aaee-123">Adding Lucidchart from hello gallery</span></span>
<span data-ttu-id="3aaee-124">tooconfigure hello integrációja Lucidchart az Azure AD-be, meg kell tooadd Lucidchart hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="3aaee-124">tooconfigure hello integration of Lucidchart into Azure AD, you need tooadd Lucidchart from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3aaee-125">**tooadd Lucidchart hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3aaee-125">**tooadd Lucidchart from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3aaee-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3aaee-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3aaee-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3aaee-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="3aaee-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3aaee-133">Hello keresési mezőbe, írja be a **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-133">In hello search box, type **Lucidchart**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="3aaee-135">A hello eredmények panelen válassza ki a **Lucidchart**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-135">In hello results panel, select **Lucidchart**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3aaee-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3aaee-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3aaee-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="3aaee-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3aaee-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Lucidchart tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3aaee-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lucidchart is tooa user in Azure AD.</span></span> <span data-ttu-id="3aaee-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Lucidchart közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="3aaee-140">In other words, a link relationship between an Azure AD user and hello related user in Lucidchart needs toobe established.</span></span>

<span data-ttu-id="3aaee-141">Lucidchart, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3aaee-141">In Lucidchart, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3aaee-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Lucidchart-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="3aaee-142">tooconfigure and test Azure AD single sign-on with Lucidchart, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3aaee-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3aaee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3aaee-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3aaee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3aaee-145">**[Lucidchart tesztfelhasználó létrehozása](#creating-a-lucidchart-test-user)**  -toohave egy megfelelője a Britta Simon a Lucidchart, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="3aaee-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - toohave a counterpart of Britta Simon in Lucidchart that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3aaee-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3aaee-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3aaee-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="3aaee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3aaee-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3aaee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3aaee-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lucidchart alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3aaee-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="3aaee-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Lucidchart, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3aaee-150">**tooconfigure Azure AD single sign-on with Lucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="3aaee-151">Az Azure portál, a hello hello **Lucidchart** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-151">In hello Azure portal, on hello **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3aaee-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3aaee-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="3aaee-155">A hello **Lucidchart tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3aaee-155">On hello **Lucidchart Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="3aaee-157">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="3aaee-157">In hello **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="3aaee-158">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3aaee-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="3aaee-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3aaee-162">Egy másik webes böngészőablakban jelentkezzen be a Lucidchart vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3aaee-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="3aaee-163">Hello hello felső menüben kattintson a **Team**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-163">In hello menu on hello top, click **Team**.</span></span>
   
    <span data-ttu-id="3aaee-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "csapata")</span><span class="sxs-lookup"><span data-stu-id="3aaee-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="3aaee-165">Kattintson a **alkalmazások \> SAML kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="3aaee-166">![SAML kezelése](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "SAML kezelése")</span><span class="sxs-lookup"><span data-stu-id="3aaee-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="3aaee-167">A hello **SAML-alapú hitelesítési beállítások** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3aaee-167">On hello **SAML Authentication Settings** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="3aaee-168">a.</span><span class="sxs-lookup"><span data-stu-id="3aaee-168">a.</span></span> <span data-ttu-id="3aaee-169">Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**, és kattintson a **nem kötelező**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="3aaee-170">![SAML-alapú hitelesítési beállítások](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML-alapú hitelesítési beállítások")</span><span class="sxs-lookup"><span data-stu-id="3aaee-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="3aaee-171">b.</span><span class="sxs-lookup"><span data-stu-id="3aaee-171">b.</span></span> <span data-ttu-id="3aaee-172">A hello **tartomány** szövegmező, írja be a tartomány, és kattintson **módosítás tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-172">In hello **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="3aaee-173">![Tanúsítvány módosítása](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "tanúsítványának módosítása")</span><span class="sxs-lookup"><span data-stu-id="3aaee-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="3aaee-174">c.</span><span class="sxs-lookup"><span data-stu-id="3aaee-174">c.</span></span> <span data-ttu-id="3aaee-175">Nyissa meg a letöltött metaadatait tartalmazó fájl, a tartalom másolás hello, és illessze be hello **metaadatok feltöltése** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3aaee-175">Open your downloaded metadata file, copy hello content, and then paste it into hello **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="3aaee-176">![Töltse fel a metaadatok](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "metaadatok feltöltése")</span><span class="sxs-lookup"><span data-stu-id="3aaee-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="3aaee-177">d.</span><span class="sxs-lookup"><span data-stu-id="3aaee-177">d.</span></span> <span data-ttu-id="3aaee-178">Válassza ki **automatikusan új felhasználók toohello csoport hozzáadása**, és kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-178">Select **Automatically Add new users toohello team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="3aaee-179">![Módosítások mentése](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "módosítások mentése")</span><span class="sxs-lookup"><span data-stu-id="3aaee-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="3aaee-180">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="3aaee-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3aaee-181">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="3aaee-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3aaee-182">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3aaee-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3aaee-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3aaee-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="3aaee-184">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="3aaee-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3aaee-186">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="3aaee-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3aaee-187">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3aaee-189">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3aaee-191">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3aaee-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3aaee-193">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3aaee-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3aaee-195">a.</span><span class="sxs-lookup"><span data-stu-id="3aaee-195">a.</span></span> <span data-ttu-id="3aaee-196">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3aaee-197">b.</span><span class="sxs-lookup"><span data-stu-id="3aaee-197">b.</span></span> <span data-ttu-id="3aaee-198">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3aaee-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3aaee-199">c.</span><span class="sxs-lookup"><span data-stu-id="3aaee-199">c.</span></span> <span data-ttu-id="3aaee-200">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3aaee-201">d.</span><span class="sxs-lookup"><span data-stu-id="3aaee-201">d.</span></span> <span data-ttu-id="3aaee-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="3aaee-203">Lucidchart tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3aaee-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="3aaee-204">Nincs a akkor tooconfigure felhasználók átadásához tooLucidchart művelet elem.</span><span class="sxs-lookup"><span data-stu-id="3aaee-204">There is no action item for you tooconfigure user provisioning tooLucidchart.</span></span>  <span data-ttu-id="3aaee-205">Ha egy hozzárendelt felhasználó megpróbál toolog Lucidchart hello hozzáférési panelen történő, Lucidchart ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="3aaee-205">When an assigned user tries toolog into Lucidchart using hello access panel, Lucidchart checks whether hello user exists.</span></span>  

<span data-ttu-id="3aaee-206">Nincs még nincs felhasználói fiók érhető el, ha a Lucidchart automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3aaee-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3aaee-207">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3aaee-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3aaee-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLucidchart megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="3aaee-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLucidchart.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3aaee-210">**tooassign Britta Simon tooLucidchart, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="3aaee-210">**tooassign Britta Simon tooLucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="3aaee-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3aaee-213">Hello alkalmazások listában válassza ki a **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-213">In hello applications list, select **Lucidchart**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="3aaee-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3aaee-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3aaee-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3aaee-217">Click **Add** button.</span></span> <span data-ttu-id="3aaee-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3aaee-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3aaee-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3aaee-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3aaee-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3aaee-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3aaee-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3aaee-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3aaee-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3aaee-223">Testing single sign-on</span></span>

<span data-ttu-id="3aaee-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="3aaee-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3aaee-225">Ha a hozzáférési Panel hello hello Lucidchart csempe gombra kattint, automatikusan bejelentkezett tooyour Lucidchart alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="3aaee-225">When you click hello Lucidchart tile in hello Access Panel, you should get automatically signed-on tooyour Lucidchart application.</span></span>
<span data-ttu-id="3aaee-226">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3aaee-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3aaee-227">További források</span><span class="sxs-lookup"><span data-stu-id="3aaee-227">Additional resources</span></span>

* [<span data-ttu-id="3aaee-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3aaee-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3aaee-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3aaee-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

