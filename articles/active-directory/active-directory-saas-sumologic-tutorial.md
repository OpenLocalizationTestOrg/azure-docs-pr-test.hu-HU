---
title: "Oktatóanyag: Azure Active Directoryval integrált SumoLogic |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SumoLogic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 2ef1bd329f5db8899f0b57744e4c0f6eed1c532f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="f1f1a-103">Oktatóanyag: Azure Active Directoryval integrált SumoLogic</span><span class="sxs-lookup"><span data-stu-id="f1f1a-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="f1f1a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SumoLogic az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f1f1a-104">In this tutorial, you learn how toointegrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f1f1a-105">SumoLogic integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-105">Integrating SumoLogic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f1f1a-106">Megadhatja a hozzáférés tooSumoLogic rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f1f1a-106">You can control in Azure AD who has access tooSumoLogic</span></span>
- <span data-ttu-id="f1f1a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSumoLogic (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f1f1a-107">You can enable your users tooautomatically get signed-on tooSumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f1f1a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f1f1a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f1f1a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f1f1a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1f1a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1f1a-110">Prerequisites</span></span>

<span data-ttu-id="f1f1a-111">az Azure AD integrálása SumoLogic tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-111">tooconfigure Azure AD integration with SumoLogic, you need hello following items:</span></span>

- <span data-ttu-id="f1f1a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f1f1a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f1f1a-113">Egy SumoLogic egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f1f1a-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f1f1a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f1f1a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f1f1a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f1f1a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1f1a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f1f1a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-118">Scenario description</span></span>
<span data-ttu-id="f1f1a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f1f1a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f1f1a-121">Hello gyűjteményből SumoLogic hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-121">Adding SumoLogic from hello gallery</span></span>
2. <span data-ttu-id="f1f1a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f1f1a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-hello-gallery"></a><span data-ttu-id="f1f1a-123">Hello gyűjteményből SumoLogic hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-123">Adding SumoLogic from hello gallery</span></span>
<span data-ttu-id="f1f1a-124">tooconfigure hello integrációja SumoLogic az Azure AD-be, meg kell tooadd SumoLogic hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-124">tooconfigure hello integration of SumoLogic into Azure AD, you need tooadd SumoLogic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f1f1a-125">**tooadd SumoLogic hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-125">**tooadd SumoLogic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1f1a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f1f1a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f1f1a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f1f1a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f1f1a-133">Hello keresési mezőbe, írja be a **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-133">In hello search box, type **SumoLogic**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="f1f1a-135">A hello eredmények panelen válassza ki a **SumoLogic**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-135">In hello results panel, select **SumoLogic**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f1f1a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f1f1a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f1f1a-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f1f1a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SumoLogic tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SumoLogic is tooa user in Azure AD.</span></span> <span data-ttu-id="f1f1a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SumoLogic közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-140">In other words, a link relationship between an Azure AD user and hello related user in SumoLogic needs toobe established.</span></span>

<span data-ttu-id="f1f1a-141">SumoLogic, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-141">In SumoLogic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f1f1a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SumoLogic-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-142">tooconfigure and test Azure AD single sign-on with SumoLogic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f1f1a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f1f1a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f1f1a-145">**[SumoLogic tesztfelhasználó létrehozása](#creating-a-sumologic-test-user)**  -toohave egy megfelelője a Britta Simon a SumoLogic, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - toohave a counterpart of Britta Simon in SumoLogic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f1f1a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f1f1a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f1f1a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f1f1a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az SumoLogic alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="f1f1a-150">**az Azure AD tooconfigure egyszeri bejelentkezést a SumoLogic, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-150">**tooconfigure Azure AD single sign-on with SumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1f1a-151">Az Azure portál, a hello hello **SumoLogic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-151">In hello Azure portal, on hello **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f1f1a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="f1f1a-155">A hello **SumoLogic tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-155">On hello **SumoLogic Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="f1f1a-157">a.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-157">a.</span></span> <span data-ttu-id="f1f1a-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="f1f1a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="f1f1a-159">b.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-159">b.</span></span> <span data-ttu-id="f1f1a-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="f1f1a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-161">These values are not real.</span></span> <span data-ttu-id="f1f1a-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f1f1a-163">Ügyfél [SumoLogic ügyfél-támogatási csoport](https://www.sumologic.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f1f1a-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="f1f1a-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f1f1a-168">A hello **SumoLogic konfigurációs** kattintson **konfigurálása SumoLogic** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-168">On hello **SumoLogic Configuration** section, click **Configure SumoLogic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f1f1a-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="f1f1a-171">Egy másik webes böngészőablakban jelentkezzen tooyour SumoLogic vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-171">In a different web browser window, log in tooyour SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="f1f1a-172">Nyissa meg túl**kezelése \> biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-172">Go too**Manage \> Security**.</span></span>
   
    <span data-ttu-id="f1f1a-173">![Kezelése](./media/active-directory-saas-sumologic-tutorial/ic778556.png "kezelése")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="f1f1a-174">Kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="f1f1a-175">![Globális biztonsági beállításokat](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globális biztonsági beállításokat")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="f1f1a-176">A hello **konfiguráció válasszon vagy hozzon létre egy új** listára, válassza ki **az Azure AD**, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-176">From hello **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="f1f1a-177">![Konfigurálja a SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "SAML 2.0 konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="f1f1a-178">A hello **konfigurálása a SAML 2.0-s** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-178">On hello **Configure SAML 2.0** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="f1f1a-179">![Konfigurálja a SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "SAML 2.0 konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="f1f1a-180">a.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-180">a.</span></span> <span data-ttu-id="f1f1a-181">A hello **Konfigurációnévvel** szövegmezőhöz típus **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-181">In hello **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="f1f1a-182">b.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-182">b.</span></span> <span data-ttu-id="f1f1a-183">Válassza ki **hibakeresési mód**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="f1f1a-184">c.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-184">c.</span></span> <span data-ttu-id="f1f1a-185">A hello **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-185">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="f1f1a-186">d.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-186">d.</span></span> <span data-ttu-id="f1f1a-187">A hello **Authn kérelem URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-187">In hello **Authn Request URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f1f1a-188">e.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-188">e.</span></span> <span data-ttu-id="f1f1a-189">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be a teljes tanúsítvány hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="f1f1a-190">f.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-190">f.</span></span> <span data-ttu-id="f1f1a-191">Mint **E-mail attribútum**, jelölje be **használható SAML tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="f1f1a-192">g.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-192">g.</span></span> <span data-ttu-id="f1f1a-193">Válassza ki **Szolgáltató kezdeményezett bejelentkezési konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="f1f1a-194">h.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-194">h.</span></span> <span data-ttu-id="f1f1a-195">A hello **bejelentkezési elérési** szövegmezőhöz típus **Azure** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-195">In hello **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f1f1a-196">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f1f1a-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f1f1a-197">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f1f1a-198">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f1f1a-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f1f1a-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="f1f1a-200">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f1f1a-202">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1f1a-203">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f1f1a-205">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f1f1a-207">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f1f1a-209">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f1f1a-211">a.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-211">a.</span></span> <span data-ttu-id="f1f1a-212">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f1f1a-213">b.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-213">b.</span></span> <span data-ttu-id="f1f1a-214">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f1f1a-215">c.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-215">c.</span></span> <span data-ttu-id="f1f1a-216">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f1f1a-217">d.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-217">d.</span></span> <span data-ttu-id="f1f1a-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="f1f1a-219">SumoLogic tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1f1a-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="f1f1a-220">Rendelés tooenable az Azure AD felhasználók toolog a tooSumoLogic kiosztott tooSumoLogic kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-220">In order tooenable Azure AD users toolog in tooSumoLogic, they must be provisioned tooSumoLogic.</span></span>  

* <span data-ttu-id="f1f1a-221">SumoLogic hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-221">In hello case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="f1f1a-222">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-222">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1f1a-223">Jelentkezzen be tooyour **SumoLogic** bérlő.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-223">Log in tooyour **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="f1f1a-224">Nyissa meg túl**kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-224">Go too**Manage \> Users**.</span></span>
   
    <span data-ttu-id="f1f1a-225">![Felhasználók](./media/active-directory-saas-sumologic-tutorial/ic778561.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="f1f1a-226">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-226">Click **Add**.</span></span>
   
    <span data-ttu-id="f1f1a-227">![Felhasználók](./media/active-directory-saas-sumologic-tutorial/ic778562.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="f1f1a-228">A hello **új felhasználó** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1f1a-228">On hello **New User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="f1f1a-229">![Új felhasználó](./media/active-directory-saas-sumologic-tutorial/ic778563.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="f1f1a-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="f1f1a-230">a.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-230">a.</span></span> <span data-ttu-id="f1f1a-231">Típus hello kapcsolódó információk tooprovision a kívánt hello hello Azure AD-fiókjának **Utónév**, **Vezetéknév**, és **E-mail** szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-231">Type hello related information of hello Azure AD account you want tooprovision into hello **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="f1f1a-232">b.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-232">b.</span></span> <span data-ttu-id="f1f1a-233">Szerepkör kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-233">Select a role.</span></span>
  
    <span data-ttu-id="f1f1a-234">c.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-234">c.</span></span> <span data-ttu-id="f1f1a-235">Mint **állapot**, jelölje be **aktív**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="f1f1a-236">d.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-236">d.</span></span> <span data-ttu-id="f1f1a-237">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="f1f1a-238">Bármely más SumoLogic felhasználói fiók létrehozása eszközök vagy SumoLogic tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f1f1a-239">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f1f1a-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f1f1a-240">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSumoLogic megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSumoLogic.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f1f1a-242">**tooassign Britta Simon tooSumoLogic, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f1f1a-242">**tooassign Britta Simon tooSumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1f1a-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f1f1a-245">Hello alkalmazások listában válassza ki a **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-245">In hello applications list, select **SumoLogic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="f1f1a-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f1f1a-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-249">Click **Add** button.</span></span> <span data-ttu-id="f1f1a-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f1f1a-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f1f1a-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f1f1a-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f1f1a-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f1f1a-255">Testing single sign-on</span></span>

<span data-ttu-id="f1f1a-256">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f1f1a-257">Ha a hozzáférési Panel hello hello SumoLogic csempe gombra kattint, automatikusan bejelentkezett tooyour SumoLogic alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f1f1a-257">When you click hello SumoLogic tile in hello Access Panel, you should get automatically signed-on tooyour SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1f1a-258">További források</span><span class="sxs-lookup"><span data-stu-id="f1f1a-258">Additional resources</span></span>

* [<span data-ttu-id="f1f1a-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f1f1a-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f1f1a-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f1f1a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

