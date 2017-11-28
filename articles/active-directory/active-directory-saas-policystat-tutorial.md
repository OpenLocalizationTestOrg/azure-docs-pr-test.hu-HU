---
title: "Oktatóanyag: Azure Active Directoryval integrált PolicyStat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PolicyStat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="a8b54-103">Oktatóanyag: Azure Active Directoryval integrált PolicyStat</span><span class="sxs-lookup"><span data-stu-id="a8b54-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="a8b54-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PolicyStat az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a8b54-104">In this tutorial, you learn how toointegrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8b54-105">PolicyStat integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-105">Integrating PolicyStat with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a8b54-106">Megadhatja a hozzáférés tooPolicyStat rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a8b54-106">You can control in Azure AD who has access tooPolicyStat</span></span>
- <span data-ttu-id="a8b54-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPolicyStat (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a8b54-107">You can enable your users tooautomatically get signed-on tooPolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a8b54-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a8b54-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a8b54-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8b54-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8b54-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a8b54-110">Prerequisites</span></span>

<span data-ttu-id="a8b54-111">az Azure AD integrálása PolicyStat tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-111">tooconfigure Azure AD integration with PolicyStat, you need hello following items:</span></span>

- <span data-ttu-id="a8b54-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a8b54-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8b54-113">Egy PolicyStat egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a8b54-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8b54-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a8b54-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8b54-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a8b54-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8b54-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a8b54-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8b54-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8b54-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8b54-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a8b54-118">Scenario description</span></span>
<span data-ttu-id="a8b54-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a8b54-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8b54-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a8b54-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8b54-121">Hello gyűjteményből PolicyStat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a8b54-121">Adding PolicyStat from hello gallery</span></span>
2. <span data-ttu-id="a8b54-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a8b54-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-hello-gallery"></a><span data-ttu-id="a8b54-123">Hello gyűjteményből PolicyStat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a8b54-123">Adding PolicyStat from hello gallery</span></span>
<span data-ttu-id="a8b54-124">tooconfigure hello integrációja PolicyStat az Azure AD-be, meg kell tooadd PolicyStat hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a8b54-124">tooconfigure hello integration of PolicyStat into Azure AD, you need tooadd PolicyStat from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8b54-125">**tooadd PolicyStat hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a8b54-125">**tooadd PolicyStat from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8b54-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a8b54-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a8b54-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a8b54-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a8b54-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a8b54-133">Hello keresési mezőbe, írja be a **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-133">In hello search box, type **PolicyStat**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="a8b54-135">A hello eredmények panelen válassza ki a **PolicyStat**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-135">In hello results panel, select **PolicyStat**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a8b54-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a8b54-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a8b54-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="a8b54-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8b54-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PolicyStat tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a8b54-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PolicyStat is tooa user in Azure AD.</span></span> <span data-ttu-id="a8b54-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PolicyStat közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a8b54-140">In other words, a link relationship between an Azure AD user and hello related user in PolicyStat needs toobe established.</span></span>

<span data-ttu-id="a8b54-141">PolicyStat, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a8b54-141">In PolicyStat, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a8b54-142">tooconfigure és az Azure AD az egyszeri bejelentkezés PolicyStat-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a8b54-142">tooconfigure and test Azure AD single sign-on with PolicyStat, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8b54-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a8b54-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8b54-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8b54-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8b54-145">**[PolicyStat tesztfelhasználó létrehozása](#creating-a-policystat-test-user)**  -toohave egy megfelelője a Britta Simon a PolicyStat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a8b54-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - toohave a counterpart of Britta Simon in PolicyStat that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8b54-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a8b54-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8b54-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a8b54-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a8b54-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a8b54-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a8b54-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PolicyStat alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a8b54-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="a8b54-150">**az Azure AD tooconfigure egyszeri bejelentkezést a PolicyStat, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a8b54-150">**tooconfigure Azure AD single sign-on with PolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8b54-151">Az Azure portál, a hello hello **PolicyStat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-151">In hello Azure portal, on hello **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a8b54-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a8b54-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="a8b54-155">A hello **PolicyStat tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-155">On hello **PolicyStat Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="a8b54-157">a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-157">a.</span></span> <span data-ttu-id="a8b54-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="a8b54-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="a8b54-159">b.</span><span class="sxs-lookup"><span data-stu-id="a8b54-159">b.</span></span> <span data-ttu-id="a8b54-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="a8b54-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a8b54-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a8b54-161">These values are not real.</span></span> <span data-ttu-id="a8b54-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="a8b54-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a8b54-163">Ügyfél [PolicyStat ügyfél-támogatási csoport](http://www.policystat.com/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a8b54-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="a8b54-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a8b54-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="a8b54-166">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooPolicyStat fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="a8b54-166">hello objective of this section is toooutline how tooenable users tooauthenticate tooPolicyStat with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="a8b54-167">hello PolicyStat alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="a8b54-167">hello PolicyStat application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="a8b54-168">a következő képernyőkép hello ez példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a8b54-168">hello following screenshot shows an example of this.</span></span>

     <span data-ttu-id="a8b54-169">![Attribútumok](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="a8b54-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="a8b54-170">tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-170">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="a8b54-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="a8b54-171">Attribute Name</span></span>    |   <span data-ttu-id="a8b54-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="a8b54-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="a8b54-173">egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="a8b54-173">uid</span></span> | <span data-ttu-id="a8b54-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="a8b54-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="a8b54-175">a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-175">a.</span></span> <span data-ttu-id="a8b54-176">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a8b54-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="a8b54-179">b.</span><span class="sxs-lookup"><span data-stu-id="a8b54-179">b.</span></span> <span data-ttu-id="a8b54-180">A hello **attribútumnév** szövegmezőhöz típus **uid**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-180">In hello **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="a8b54-181">c.</span><span class="sxs-lookup"><span data-stu-id="a8b54-181">c.</span></span> <span data-ttu-id="a8b54-182">A hello **attribútumérték** szövegmező, jelölje be **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-182">In hello **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="a8b54-183">d.</span><span class="sxs-lookup"><span data-stu-id="a8b54-183">d.</span></span> <span data-ttu-id="a8b54-184">A hello **Mail** listáról válassza ki **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-184">From hello **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="a8b54-185">e.</span><span class="sxs-lookup"><span data-stu-id="a8b54-185">e.</span></span> <span data-ttu-id="a8b54-186">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="a8b54-186">Click **Ok**</span></span>

7. <span data-ttu-id="a8b54-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a8b54-189">Egy másik webes böngészőablakban jelentkezzen be a PolicyStat vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a8b54-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="a8b54-190">Hello kattintson **Admin** fülre, majd **egyszeri bejelentkezés konfigurációs** bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a8b54-190">Click hello **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="a8b54-191">![Rendszergazda menü](./media/active-directory-saas-policystat-tutorial/ic808633.png "Rendszergazda menü")</span><span class="sxs-lookup"><span data-stu-id="a8b54-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="a8b54-192">A hello **telepítő** szakaszban jelölje be **engedélyezése egyszeri bejelentkezéshez integrációs**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-192">In hello **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="a8b54-193">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808634.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a8b54-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="a8b54-194">Kattintson a **attribútumok konfigurálása**, majd a hello **attribútumok konfigurálása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-194">Click **Configure Attributes**, and then, in hello **Configure Attributes** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a8b54-195">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808635.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a8b54-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="a8b54-196">a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-196">a.</span></span> <span data-ttu-id="a8b54-197">A hello **felhasználónév attribútum** szövegmezőhöz típus **uid**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-197">In hello **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="a8b54-198">b.</span><span class="sxs-lookup"><span data-stu-id="a8b54-198">b.</span></span> <span data-ttu-id="a8b54-199">A hello **Keresztnév attribútum** szövegmezőhöz típus **Keresztnév** felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-199">In hello **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="a8b54-200">c.</span><span class="sxs-lookup"><span data-stu-id="a8b54-200">c.</span></span> <span data-ttu-id="a8b54-201">A hello **utolsó Name attribútum** szövegmezőhöz típus **Vezetéknév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-201">In hello **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="a8b54-202">d.</span><span class="sxs-lookup"><span data-stu-id="a8b54-202">d.</span></span> <span data-ttu-id="a8b54-203">A hello **E-mail attribútum** szövegmezőhöz típus **emailaddress** felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a8b54-203">In hello **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="a8b54-204">e.</span><span class="sxs-lookup"><span data-stu-id="a8b54-204">e.</span></span> <span data-ttu-id="a8b54-205">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="a8b54-206">Kattintson a **a kiállító terjesztési hely metaadatok**, majd a hello **a kiállító terjesztési hely metaadatok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-206">Click **Your IDP Metadata**, and then, in hello **Your IDP Metadata** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a8b54-207">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808636.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a8b54-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="a8b54-208">a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-208">a.</span></span> <span data-ttu-id="a8b54-209">Nyissa meg a letöltött metaadatait tartalmazó fájl, a tartalom másolás hello, és illessze be hello **a Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a8b54-209">Open your downloaded metadata file, copy hello content, and  then paste it into hello **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="a8b54-210">b.</span><span class="sxs-lookup"><span data-stu-id="a8b54-210">b.</span></span> <span data-ttu-id="a8b54-211">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="a8b54-212">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a8b54-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a8b54-213">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a8b54-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a8b54-214">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8b54-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a8b54-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a8b54-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="a8b54-216">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a8b54-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a8b54-218">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a8b54-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8b54-219">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a8b54-221">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a8b54-223">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a8b54-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a8b54-225">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b54-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a8b54-227">a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-227">a.</span></span> <span data-ttu-id="a8b54-228">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8b54-229">b.</span><span class="sxs-lookup"><span data-stu-id="a8b54-229">b.</span></span> <span data-ttu-id="a8b54-230">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a8b54-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a8b54-231">c.</span><span class="sxs-lookup"><span data-stu-id="a8b54-231">c.</span></span> <span data-ttu-id="a8b54-232">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a8b54-233">d.</span><span class="sxs-lookup"><span data-stu-id="a8b54-233">d.</span></span> <span data-ttu-id="a8b54-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="a8b54-235">PolicyStat tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a8b54-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="a8b54-236">A sorrend tooenable az Azure AD felhasználók toolog PolicyStat be azok ki kell építenie PolicyStat be.</span><span class="sxs-lookup"><span data-stu-id="a8b54-236">In order tooenable Azure AD users toolog into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="a8b54-237">PolicyStat csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="a8b54-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="a8b54-238">Ez azt jelenti, tooadd hello felhasználóknak nem kell manuálisan tooPolicyStat.</span><span class="sxs-lookup"><span data-stu-id="a8b54-238">This means, you do not need tooadd hello users manually tooPolicyStat.</span></span> <span data-ttu-id="a8b54-239">hello felhasználókat a rendszer beolvasása adja hozzá automatikusan az első bejelentkezés SSO keresztül a.</span><span class="sxs-lookup"><span data-stu-id="a8b54-239">hello users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="a8b54-240">Bármely más PolicyStat felhasználói fiók létrehozása eszközök vagy PolicyStat tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="a8b54-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a8b54-241">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a8b54-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a8b54-242">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPolicyStat megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a8b54-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPolicyStat.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a8b54-244">**tooassign Britta Simon tooPolicyStat, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a8b54-244">**tooassign Britta Simon tooPolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8b54-245">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a8b54-247">Hello alkalmazások listában válassza ki a **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-247">In hello applications list, select **PolicyStat**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="a8b54-249">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a8b54-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a8b54-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a8b54-251">Click **Add** button.</span></span> <span data-ttu-id="a8b54-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a8b54-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a8b54-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a8b54-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a8b54-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a8b54-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8b54-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a8b54-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a8b54-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a8b54-257">Testing single sign-on</span></span>

<span data-ttu-id="a8b54-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a8b54-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a8b54-259">Ha a hozzáférési Panel hello hello PolicyStat csempe gombra kattint, automatikusan bejelentkezett tooyour PolicyStat alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a8b54-259">When you click hello PolicyStat tile in hello Access Panel, you should get automatically signed-on tooyour PolicyStat application.</span></span>
<span data-ttu-id="a8b54-260">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a8b54-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8b54-261">További források</span><span class="sxs-lookup"><span data-stu-id="a8b54-261">Additional resources</span></span>

* [<span data-ttu-id="a8b54-262">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a8b54-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8b54-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a8b54-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

