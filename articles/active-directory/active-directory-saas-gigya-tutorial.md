---
title: "Oktatóanyag: Azure Active Directoryval integrált Gigya |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Gigya között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 1992c5ad09b097563377a488fbf5a375f6511020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a><span data-ttu-id="1ed5d-103">Oktatóanyag: Azure Active Directoryval integrált Gigya</span><span class="sxs-lookup"><span data-stu-id="1ed5d-103">Tutorial: Azure Active Directory integration with Gigya</span></span>

<span data-ttu-id="1ed5d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Gigya az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1ed5d-104">In this tutorial, you learn how toointegrate Gigya with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1ed5d-105">Gigya integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-105">Integrating Gigya with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1ed5d-106">Megadhatja a hozzáférés tooGigya rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1ed5d-106">You can control in Azure AD who has access tooGigya</span></span>
- <span data-ttu-id="1ed5d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGigya (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1ed5d-107">You can enable your users tooautomatically get signed-on tooGigya (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1ed5d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1ed5d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1ed5d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1ed5d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ed5d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1ed5d-110">Prerequisites</span></span>

<span data-ttu-id="1ed5d-111">az Azure AD integrálása Gigya tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-111">tooconfigure Azure AD integration with Gigya, you need hello following items:</span></span>

- <span data-ttu-id="1ed5d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1ed5d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1ed5d-113">Egy Gigya egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1ed5d-113">A Gigya single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1ed5d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1ed5d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1ed5d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1ed5d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1ed5d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1ed5d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-118">Scenario description</span></span>
<span data-ttu-id="1ed5d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1ed5d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1ed5d-121">Hello gyűjteményből Gigya hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-121">Adding Gigya from hello gallery</span></span>
2. <span data-ttu-id="1ed5d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1ed5d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gigya-from-hello-gallery"></a><span data-ttu-id="1ed5d-123">Hello gyűjteményből Gigya hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-123">Adding Gigya from hello gallery</span></span>
<span data-ttu-id="1ed5d-124">tooconfigure hello integrációja Gigya az Azure AD-be, meg kell tooadd Gigya hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-124">tooconfigure hello integration of Gigya into Azure AD, you need tooadd Gigya from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1ed5d-125">**tooadd Gigya hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1ed5d-125">**tooadd Gigya from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1ed5d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1ed5d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1ed5d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1ed5d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1ed5d-133">Hello keresési mezőbe, írja be a **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-133">In hello search box, type **Gigya**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. <span data-ttu-id="1ed5d-135">A hello eredmények panelen válassza ki a **Gigya**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-135">In hello results panel, select **Gigya**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1ed5d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1ed5d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1ed5d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Gigya.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-138">In this section, you configure and test Azure AD single sign-on with Gigya based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1ed5d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Gigya tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Gigya is tooa user in Azure AD.</span></span> <span data-ttu-id="1ed5d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Gigya közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-140">In other words, a link relationship between an Azure AD user and hello related user in Gigya needs toobe established.</span></span>

<span data-ttu-id="1ed5d-141">Gigya, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-141">In Gigya, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1ed5d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Gigya-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-142">tooconfigure and test Azure AD single sign-on with Gigya, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1ed5d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1ed5d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1ed5d-145">**[Gigya tesztfelhasználó létrehozása](#creating-a-gigya-test-user)**  -toohave egy megfelelője a Britta Simon a Gigya, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-145">**[Creating a Gigya test user](#creating-a-gigya-test-user)** - toohave a counterpart of Britta Simon in Gigya that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1ed5d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1ed5d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1ed5d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1ed5d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Gigya alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Gigya application.</span></span>

<span data-ttu-id="1ed5d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Gigya, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1ed5d-150">**tooconfigure Azure AD single sign-on with Gigya, perform hello following steps:**</span></span>

1. <span data-ttu-id="1ed5d-151">Az Azure portál, a hello hello **Gigya** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-151">In hello Azure portal, on hello **Gigya** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1ed5d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. <span data-ttu-id="1ed5d-155">A hello **Gigya tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-155">On hello **Gigya Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    <span data-ttu-id="1ed5d-157">a.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-157">a.</span></span> <span data-ttu-id="1ed5d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<companyname>.gigya.com`</span><span class="sxs-lookup"><span data-stu-id="1ed5d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<companyname>.gigya.com`</span></span>

    <span data-ttu-id="1ed5d-159">b.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-159">b.</span></span> <span data-ttu-id="1ed5d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://fidm.gigya.com/saml/v2.0/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="1ed5d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://fidm.gigya.com/saml/v2.0/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1ed5d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-161">These values are not real.</span></span> <span data-ttu-id="1ed5d-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1ed5d-163">Ügyfél [Gigya ügyfél-támogatási csoport](https://www.gigya.com/support-policy/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-163">Contact [Gigya Client support team](https://www.gigya.com/support-policy/) tooget these values.</span></span> 
 
4. <span data-ttu-id="1ed5d-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. <span data-ttu-id="1ed5d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1ed5d-168">A hello **Gigya konfigurációs** kattintson **konfigurálása Gigya** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-168">On hello **Gigya Configuration** section, click **Configure Gigya** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1ed5d-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1ed5d-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. <span data-ttu-id="1ed5d-171">Egy másik webes böngészőablakban jelentkezzen be a Gigya vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-171">In a different web browser window, log into your Gigya company site as an administrator.</span></span>

8. <span data-ttu-id="1ed5d-172">Nyissa meg túl**beállítások \> SAML bejelentkezési**, és kattintson a hello **hozzáadása** gomb.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-172">Go too**Settings \> SAML Login**, and then click hello **Add** button.</span></span>
   
    <span data-ttu-id="1ed5d-173">![SAML bejelentkezési](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML-bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="1ed5d-173">![SAML Login](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML Login")</span></span>

9. <span data-ttu-id="1ed5d-174">A hello **SAML bejelentkezési** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-174">In hello **SAML Login** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="1ed5d-175">![SAML-alapú konfigurációs](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="1ed5d-175">![SAML Configuration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML Configuration")</span></span>
   
    <span data-ttu-id="1ed5d-176">a.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-176">a.</span></span> <span data-ttu-id="1ed5d-177">A hello **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-177">In hello **Name** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="1ed5d-178">b.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-178">b.</span></span> <span data-ttu-id="1ed5d-179">A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-179">In **Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure Portal.</span></span> 
   
    <span data-ttu-id="1ed5d-180">c.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-180">c.</span></span> <span data-ttu-id="1ed5d-181">A **egyszeri bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-181">In **Single Sign-On Service URL** textbox, paste hello value of **Single Sign-On Service URL** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="1ed5d-182">d.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-182">d.</span></span> <span data-ttu-id="1ed5d-183">A **név Azonosítóformátumának** szövegmezőhöz Beillesztés hello értékének **azonosító formátuma** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-183">In **Name ID Format** textbox, paste hello value of **Name Identifier Format** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="1ed5d-184">e.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-184">e.</span></span> <span data-ttu-id="1ed5d-185">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-185">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="1ed5d-186">f.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-186">f.</span></span> <span data-ttu-id="1ed5d-187">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-187">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="1ed5d-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="1ed5d-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1ed5d-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1ed5d-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1ed5d-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1ed5d-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="1ed5d-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1ed5d-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1ed5d-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1ed5d-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1ed5d-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1ed5d-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1ed5d-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1ed5d-203">a.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-203">a.</span></span> <span data-ttu-id="1ed5d-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1ed5d-205">b.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-205">b.</span></span> <span data-ttu-id="1ed5d-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1ed5d-207">c.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-207">c.</span></span> <span data-ttu-id="1ed5d-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1ed5d-209">d.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-209">d.</span></span> <span data-ttu-id="1ed5d-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-210">Click **Create**.</span></span>
 
### <a name="creating-a-gigya-test-user"></a><span data-ttu-id="1ed5d-211">Gigya tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed5d-211">Creating a Gigya test user</span></span>

<span data-ttu-id="1ed5d-212">A sorrend tooenable az Azure AD felhasználók toolog Gigya be azok ki kell építenie Gigya be.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-212">In order tooenable Azure AD users toolog into Gigya, they must be provisioned into Gigya.</span></span>  
<span data-ttu-id="1ed5d-213">Gigya hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-213">In hello case of Gigya, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="1ed5d-214">tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-214">tooprovision a user accounts, perform hello following steps:</span></span>

1. <span data-ttu-id="1ed5d-215">Jelentkezzen be tooyour **Gigya** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-215">Log in tooyour **Gigya** company site as an administrator.</span></span>

2. <span data-ttu-id="1ed5d-216">Nyissa meg túl**Admin \> felhasználók kezelése**, és kattintson a **meghívása felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-216">Go too**Admin \> Manage Users**, and then click **Invite Users**.</span></span>
   
    <span data-ttu-id="1ed5d-217">![Felhasználók kezelése](./media/active-directory-saas-gigya-tutorial/ic789535.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="1ed5d-217">![Manage Users](./media/active-directory-saas-gigya-tutorial/ic789535.png "Manage Users")</span></span>

3. <span data-ttu-id="1ed5d-218">Hello meghívása felhasználók párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="1ed5d-218">On hello Invite Users dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="1ed5d-219">![Meghívott felhasználóknak](./media/active-directory-saas-gigya-tutorial/ic789536.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="1ed5d-219">![Invite Users](./media/active-directory-saas-gigya-tutorial/ic789536.png "Invite Users")</span></span>
   
    <span data-ttu-id="1ed5d-220">a.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-220">a.</span></span> <span data-ttu-id="1ed5d-221">A hello **E-mail** szövegmező, egy érvényes Azure Active Directory-fiókot szeretne tooprovision típus hello e-mail aliasát.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-221">In hello **Email** textbox, type hello email alias of a valid Azure Active Directory account you want tooprovision.</span></span>
    
    <span data-ttu-id="1ed5d-222">b.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-222">b.</span></span> <span data-ttu-id="1ed5d-223">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-223">Click **Invite User**.</span></span>
      
    > [!NOTE]
    > <span data-ttu-id="1ed5d-224">hello Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-224">hello Azure Active Directory account holder will receive an email that includes a link tooconfirm hello account before it becomes active.</span></span>
    > 
    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1ed5d-225">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1ed5d-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1ed5d-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooGigya megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGigya.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1ed5d-228">**tooassign Britta Simon tooGigya, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1ed5d-228">**tooassign Britta Simon tooGigya, perform hello following steps:**</span></span>

1. <span data-ttu-id="1ed5d-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1ed5d-231">Hello alkalmazások listában válassza ki a **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-231">In hello applications list, select **Gigya**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. <span data-ttu-id="1ed5d-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1ed5d-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-235">Click **Add** button.</span></span> <span data-ttu-id="1ed5d-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1ed5d-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1ed5d-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1ed5d-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1ed5d-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1ed5d-241">Testing single sign-on</span></span>

<span data-ttu-id="1ed5d-242">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="1ed5d-243">Ha a hozzáférési Panel hello hello Gigya csempe gombra kattint, automatikusan bejelentkezett tooyour Gigya alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="1ed5d-243">When you click hello Gigya tile in hello Access Panel, you should get automatically signed-on tooyour Gigya application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ed5d-244">További források</span><span class="sxs-lookup"><span data-stu-id="1ed5d-244">Additional resources</span></span>

* [<span data-ttu-id="1ed5d-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1ed5d-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1ed5d-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1ed5d-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

