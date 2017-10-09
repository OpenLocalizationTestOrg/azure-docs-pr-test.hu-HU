---
title: "Oktatóanyag: Azure Active Directoryval integrált cukor CRM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és cukor CRM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 108d2f8125e410743ee7bc48883a1d0b00602615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="b1d7b-103">Oktatóanyag: Azure Active Directoryval integrált cukor CRM</span><span class="sxs-lookup"><span data-stu-id="b1d7b-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="b1d7b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate cukor CRM, Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1d7b-104">In this tutorial, you learn how toointegrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1d7b-105">Cukor CRM integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-105">Integrating Sugar CRM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1d7b-106">Megadhatja a hozzáférés tooSugar CRM rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b1d7b-106">You can control in Azure AD who has access tooSugar CRM</span></span>
- <span data-ttu-id="b1d7b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSugar CRM (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b1d7b-107">You can enable your users tooautomatically get signed-on tooSugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1d7b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b1d7b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b1d7b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1d7b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1d7b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b1d7b-110">Prerequisites</span></span>

<span data-ttu-id="b1d7b-111">tooconfigure cukor CRM az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-111">tooconfigure Azure AD integration with Sugar CRM, you need hello following items:</span></span>

- <span data-ttu-id="b1d7b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b1d7b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1d7b-113">Egy cukor CRM egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b1d7b-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1d7b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1d7b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1d7b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1d7b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1d7b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1d7b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-118">Scenario description</span></span>
<span data-ttu-id="b1d7b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1d7b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1d7b-121">Hello gyűjteményből cukor CRM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-121">Adding Sugar CRM from hello gallery</span></span>
2. <span data-ttu-id="b1d7b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1d7b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-hello-gallery"></a><span data-ttu-id="b1d7b-123">Hello gyűjteményből cukor CRM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-123">Adding Sugar CRM from hello gallery</span></span>
<span data-ttu-id="b1d7b-124">tooconfigure hello integrációs cukor CRM az Azure AD-be, meg kell tooadd cukor CRM hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-124">tooconfigure hello integration of Sugar CRM into Azure AD, you need tooadd Sugar CRM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1d7b-125">**tooadd cukor CRM hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-125">**tooadd Sugar CRM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1d7b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1d7b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1d7b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b1d7b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b1d7b-133">Hello keresési mezőbe, írja be a **cukor CRM**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-133">In hello search box, type **Sugar CRM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="b1d7b-135">A hello eredmények panelen válassza ki a **cukor CRM**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-135">In hello results panel, select **Sugar CRM**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1d7b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1d7b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1d7b-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés cukor CRM "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b1d7b-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói cukor CRM tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sugar CRM is tooa user in Azure AD.</span></span> <span data-ttu-id="b1d7b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello cukor CRM közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-140">In other words, a link relationship between an Azure AD user and hello related user in Sugar CRM needs toobe established.</span></span>

<span data-ttu-id="b1d7b-141">A cukor CRM rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-141">In Sugar CRM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b1d7b-142">tooconfigure és az Azure AD az egyszeri bejelentkezés cukor CRM-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-142">tooconfigure and test Azure AD single sign-on with Sugar CRM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1d7b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1d7b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1d7b-145">**[Cukor CRM tesztfelhasználó létrehozása](#creating-a-sugar-crm-test-user)**  -toohave egy megfelelője a Britta Simon a csatolt toohello az Azure AD felhasználói ábrázolása cukor CRM-ben.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - toohave a counterpart of Britta Simon in Sugar CRM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b1d7b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1d7b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1d7b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1d7b-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az cukor CRM alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="b1d7b-150">**az Azure AD tooconfigure egyszeri bejelentkezés cukor CRM, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-150">**tooconfigure Azure AD single sign-on with Sugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1d7b-151">Az Azure portál, a hello hello **cukor CRM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-151">In hello Azure portal, on hello **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b1d7b-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="b1d7b-155">A hello **cukor CRM tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-155">On hello **Sugar CRM Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="b1d7b-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="b1d7b-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-158">hello value is not real.</span></span> <span data-ttu-id="b1d7b-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b1d7b-160">Ügyfél [cukor CRM ügyfél-támogatási csoport](https://support.sugarcrm.com/) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="b1d7b-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="b1d7b-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b1d7b-165">A hello **cukor CRM konfigurációs** kattintson **cukor CRM konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-165">On hello **Sugar CRM Configuration** section, click **Configure Sugar CRM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b1d7b-166">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="b1d7b-168">Egy másik webes böngészőablakban jelentkezzen tooyour cukor CRM vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-168">In a different web browser window, log in tooyour Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="b1d7b-169">Nyissa meg túl**Admin**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-169">Go too**Admin**.</span></span>
   
    <span data-ttu-id="b1d7b-170">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="b1d7b-171">A hello **felügyeleti** kattintson **jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-171">In hello **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="b1d7b-172">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="b1d7b-173">Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="b1d7b-174">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="b1d7b-175">A hello **SAML-alapú hitelesítés** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-175">In hello **SAML Authentication** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1d7b-176">![SAML-alapú hitelesítés](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="b1d7b-177">a.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-177">a.</span></span> <span data-ttu-id="b1d7b-178">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-178">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="b1d7b-179">b.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-179">b.</span></span> <span data-ttu-id="b1d7b-180">A hello **SLO URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-180">In hello **SLO URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="b1d7b-181">c.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-181">c.</span></span> <span data-ttu-id="b1d7b-182">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be a teljes tanúsítvány hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-182">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="b1d7b-183">d.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-183">d.</span></span> <span data-ttu-id="b1d7b-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="b1d7b-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b1d7b-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b1d7b-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b1d7b-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b1d7b-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1d7b-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1d7b-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b1d7b-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1d7b-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1d7b-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1d7b-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1d7b-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1d7b-200">a.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-200">a.</span></span> <span data-ttu-id="b1d7b-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1d7b-202">b.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-202">b.</span></span> <span data-ttu-id="b1d7b-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1d7b-204">c.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-204">c.</span></span> <span data-ttu-id="b1d7b-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1d7b-206">d.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-206">d.</span></span> <span data-ttu-id="b1d7b-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="b1d7b-208">Cukor CRM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1d7b-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="b1d7b-209">Rendelés tooenable az Azure AD felhasználók toolog a tooSugar CRM kiosztott tooSugar CRM kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-209">In order tooenable Azure AD users toolog in tooSugar CRM, they must be provisioned tooSugar CRM.</span></span>

<span data-ttu-id="b1d7b-210">Cukor CRM hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-210">In hello case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="b1d7b-211">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1d7b-212">Jelentkezzen be tooyour **cukor CRM** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-212">Log in tooyour **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="b1d7b-213">Nyissa meg túl**Admin**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-213">Go too**Admin**.</span></span>
   
    <span data-ttu-id="b1d7b-214">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="b1d7b-215">A hello **felügyeleti** kattintson **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-215">In hello **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="b1d7b-216">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="b1d7b-217">Nyissa meg túl**felhasználók \> új felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-217">Go too**Users \> Create New User**.</span></span>
   
    <span data-ttu-id="b1d7b-218">![Új felhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "új felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="b1d7b-219">A hello **felhasználói profil** lapra, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-219">On hello **User Profile** tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1d7b-220">![Új felhasználó](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="b1d7b-221">a.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-221">a.</span></span> <span data-ttu-id="b1d7b-222">Típus hello **felhasználónév**, **Vezetéknév**, és **e-mail cím** hello be egy érvényes Azure Active Directory felhasználó kapcsolatos szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-222">Type hello **user name**, **last name**, and **email address** of a valid Azure Active Directory user into hello related textboxes.</span></span>
  
6. <span data-ttu-id="b1d7b-223">Mint **állapot**, jelölje be **aktív**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="b1d7b-224">Hello jelszó lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b1d7b-224">On hello Password tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1d7b-225">![Új felhasználó](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="b1d7b-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="b1d7b-226">a.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-226">a.</span></span> <span data-ttu-id="b1d7b-227">Írja be a jelszót hello hello be kapcsolódó szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-227">Type hello password into hello related textbox.</span></span>

    <span data-ttu-id="b1d7b-228">b.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-228">b.</span></span> <span data-ttu-id="b1d7b-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="b1d7b-230">Bármely más cukor CRM felhasználói fiók létrehozása eszközök vagy cukor CRM tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b1d7b-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b1d7b-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b1d7b-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSugar CRM megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSugar CRM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b1d7b-234">**tooassign Britta Simon tooSugar CRM, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b1d7b-234">**tooassign Britta Simon tooSugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1d7b-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b1d7b-237">Hello alkalmazások listában válassza ki a **cukor CRM**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-237">In hello applications list, select **Sugar CRM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="b1d7b-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b1d7b-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-241">Click **Add** button.</span></span> <span data-ttu-id="b1d7b-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b1d7b-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1d7b-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1d7b-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1d7b-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b1d7b-247">Testing single sign-on</span></span>

<span data-ttu-id="b1d7b-248">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-248">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1d7b-249">Ha hello cukor CRM csempe a hozzáférési Panel hello gombra kattint, kapja meg automatikusan bejelentkezett tooyour cukor CRM-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b1d7b-249">When you click hello Sugar CRM tile in hello Access Panel, you should get automatically signed-on tooyour Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1d7b-250">További források</span><span class="sxs-lookup"><span data-stu-id="b1d7b-250">Additional resources</span></span>

* [<span data-ttu-id="b1d7b-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b1d7b-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1d7b-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b1d7b-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

