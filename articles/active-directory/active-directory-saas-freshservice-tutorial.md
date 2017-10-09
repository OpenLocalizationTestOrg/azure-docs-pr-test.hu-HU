---
title: "Oktatóanyag: Azure Active Directoryval integrált Freshservice |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Freshservice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a><span data-ttu-id="193f2-103">Oktatóanyag: Azure Active Directoryval integrált Freshservice</span><span class="sxs-lookup"><span data-stu-id="193f2-103">Tutorial: Azure Active Directory integration with Freshservice</span></span>

<span data-ttu-id="193f2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Freshservice az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="193f2-104">In this tutorial, you learn how toointegrate Freshservice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="193f2-105">Freshservice integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-105">Integrating Freshservice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="193f2-106">Megadhatja a hozzáférés tooFreshservice rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="193f2-106">You can control in Azure AD who has access tooFreshservice</span></span>
- <span data-ttu-id="193f2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFreshservice (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="193f2-107">You can enable your users tooautomatically get signed-on tooFreshservice (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="193f2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="193f2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="193f2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="193f2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="193f2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="193f2-110">Prerequisites</span></span>

<span data-ttu-id="193f2-111">az Azure AD integrálása Freshservice tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-111">tooconfigure Azure AD integration with Freshservice, you need hello following items:</span></span>

- <span data-ttu-id="193f2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="193f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="193f2-113">Egy Freshservice egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="193f2-113">A Freshservice single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="193f2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="193f2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="193f2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="193f2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="193f2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="193f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="193f2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="193f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="193f2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="193f2-118">Scenario description</span></span>
<span data-ttu-id="193f2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="193f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="193f2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="193f2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="193f2-121">Hello gyűjteményből Freshservice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="193f2-121">Adding Freshservice from hello gallery</span></span>
2. <span data-ttu-id="193f2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="193f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshservice-from-hello-gallery"></a><span data-ttu-id="193f2-123">Hello gyűjteményből Freshservice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="193f2-123">Adding Freshservice from hello gallery</span></span>
<span data-ttu-id="193f2-124">tooconfigure hello integrációja Freshservice az Azure AD-be, meg kell tooadd Freshservice hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="193f2-124">tooconfigure hello integration of Freshservice into Azure AD, you need tooadd Freshservice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="193f2-125">**tooadd Freshservice hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="193f2-125">**tooadd Freshservice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="193f2-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="193f2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="193f2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="193f2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="193f2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="193f2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="193f2-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="193f2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="193f2-133">Hello keresési mezőbe, írja be a **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="193f2-133">In hello search box, type **Freshservice**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. <span data-ttu-id="193f2-135">A hello eredmények panelen válassza ki a **Freshservice**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="193f2-135">In hello results panel, select **Freshservice**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="193f2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="193f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="193f2-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Freshservice.</span><span class="sxs-lookup"><span data-stu-id="193f2-138">In this section, you configure and test Azure AD single sign-on with Freshservice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="193f2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Freshservice tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="193f2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Freshservice is tooa user in Azure AD.</span></span> <span data-ttu-id="193f2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Freshservice közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="193f2-140">In other words, a link relationship between an Azure AD user and hello related user in Freshservice needs toobe established.</span></span>

<span data-ttu-id="193f2-141">Freshservice, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="193f2-141">In Freshservice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="193f2-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Freshservice-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="193f2-142">tooconfigure and test Azure AD single sign-on with Freshservice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="193f2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="193f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="193f2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="193f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="193f2-145">**[Freshservice tesztfelhasználó létrehozása](#creating-a-freshservice-test-user)**  -toohave egy megfelelője a Britta Simon a Freshservice, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="193f2-145">**[Creating a Freshservice test user](#creating-a-freshservice-test-user)** - toohave a counterpart of Britta Simon in Freshservice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="193f2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="193f2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="193f2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="193f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="193f2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="193f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="193f2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Freshservice alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="193f2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Freshservice application.</span></span>

<span data-ttu-id="193f2-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Freshservice, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="193f2-150">**tooconfigure Azure AD single sign-on with Freshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="193f2-151">Az Azure portál, a hello hello **Freshservice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="193f2-151">In hello Azure portal, on hello **Freshservice** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="193f2-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="193f2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. <span data-ttu-id="193f2-155">A hello **Freshservice tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-155">On hello **Freshservice Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    <span data-ttu-id="193f2-157">a.</span><span class="sxs-lookup"><span data-stu-id="193f2-157">a.</span></span> <span data-ttu-id="193f2-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="193f2-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    <span data-ttu-id="193f2-159">b.</span><span class="sxs-lookup"><span data-stu-id="193f2-159">b.</span></span> <span data-ttu-id="193f2-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="193f2-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="193f2-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="193f2-161">These values are not real.</span></span> <span data-ttu-id="193f2-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="193f2-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="193f2-163">Ügyfél [Freshservice ügyfél-támogatási csoport](https://support.freshservice.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="193f2-163">Contact [Freshservice Client support team](https://support.freshservice.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="193f2-164">A hello **SAML-aláíró tanúsítványa** területen másolása **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="193f2-164">On hello **SAML Signing Certificate** section, copy **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. <span data-ttu-id="193f2-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="193f2-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="193f2-168">A hello **Freshservice konfigurációs** kattintson **konfigurálása Freshservice** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="193f2-168">On hello **Freshservice Configuration** section, click **Configure Freshservice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="193f2-169">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="193f2-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. <span data-ttu-id="193f2-171">Egy másik webes böngészőablakban jelentkezzen tooyour Freshservice vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="193f2-171">In a different web browser window, log in tooyour Freshservice company site as an administrator.</span></span>

8. <span data-ttu-id="193f2-172">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="193f2-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="193f2-173">![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="193f2-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

9. <span data-ttu-id="193f2-174">A hello **Ügyfélportálra**, kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="193f2-174">In hello **Customer Portal**, click **Security**.</span></span>
   
    <span data-ttu-id="193f2-175">![Biztonsági](./media/active-directory-saas-freshservice-tutorial/ic790815.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="193f2-175">![Security](./media/active-directory-saas-freshservice-tutorial/ic790815.png "Security")</span></span>

10. <span data-ttu-id="193f2-176">A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-176">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="193f2-177">![Egyszeri bejelentkezés](./media/active-directory-saas-freshservice-tutorial/ic790816.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="193f2-177">![Single Sign On](./media/active-directory-saas-freshservice-tutorial/ic790816.png "Single Sign On")</span></span>
   
    <span data-ttu-id="193f2-178">a.</span><span class="sxs-lookup"><span data-stu-id="193f2-178">a.</span></span> <span data-ttu-id="193f2-179">Kapcsoló **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="193f2-179">Switch **Single Sign On**.</span></span>

    <span data-ttu-id="193f2-180">b.</span><span class="sxs-lookup"><span data-stu-id="193f2-180">b.</span></span> <span data-ttu-id="193f2-181">Válassza ki **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="193f2-181">Select **SAML SSO**.</span></span>

    <span data-ttu-id="193f2-182">c.</span><span class="sxs-lookup"><span data-stu-id="193f2-182">c.</span></span> <span data-ttu-id="193f2-183">A hello **SAML bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="193f2-183">In hello **SAML Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="193f2-184">d.</span><span class="sxs-lookup"><span data-stu-id="193f2-184">d.</span></span> <span data-ttu-id="193f2-185">A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="193f2-185">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="193f2-186">e.</span><span class="sxs-lookup"><span data-stu-id="193f2-186">e.</span></span> <span data-ttu-id="193f2-187">A **biztonsági tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **UJJLENYOMAT** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="193f2-187">In **Security Certificate Fingerprint** textbox, paste hello **THUMBPRINT** value of certificate which you have copied from Azure portal.</span></span>

    <span data-ttu-id="193f2-188">f.</span><span class="sxs-lookup"><span data-stu-id="193f2-188">f.</span></span> <span data-ttu-id="193f2-189">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="193f2-189">Click **Save**</span></span>
   
> [!TIP]
> <span data-ttu-id="193f2-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="193f2-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="193f2-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="193f2-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="193f2-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="193f2-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="193f2-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="193f2-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="193f2-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="193f2-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="193f2-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="193f2-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="193f2-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="193f2-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="193f2-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="193f2-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="193f2-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="193f2-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="193f2-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="193f2-205">a.</span><span class="sxs-lookup"><span data-stu-id="193f2-205">a.</span></span> <span data-ttu-id="193f2-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="193f2-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="193f2-207">b.</span><span class="sxs-lookup"><span data-stu-id="193f2-207">b.</span></span> <span data-ttu-id="193f2-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="193f2-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="193f2-209">c.</span><span class="sxs-lookup"><span data-stu-id="193f2-209">c.</span></span> <span data-ttu-id="193f2-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="193f2-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="193f2-211">d.</span><span class="sxs-lookup"><span data-stu-id="193f2-211">d.</span></span> <span data-ttu-id="193f2-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="193f2-212">Click **Create**.</span></span>
 
### <a name="creating-a-freshservice-test-user"></a><span data-ttu-id="193f2-213">Freshservice tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="193f2-213">Creating a Freshservice test user</span></span>

<span data-ttu-id="193f2-214">az Azure AD tooenable felhasználók toolog a tooFreshService, akkor ki kell építenie FreshService be.</span><span class="sxs-lookup"><span data-stu-id="193f2-214">tooenable Azure AD users toolog in tooFreshService, they must be provisioned into FreshService.</span></span> <span data-ttu-id="193f2-215">FreshService hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="193f2-215">In hello case of FreshService, provisioning is a manual task.</span></span>

<span data-ttu-id="193f2-216">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="193f2-216">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="193f2-217">Jelentkezzen be tooyour **FreshService** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="193f2-217">Log in tooyour **FreshService** company site as an administrator.</span></span>

2. <span data-ttu-id="193f2-218">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="193f2-218">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="193f2-219">![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="193f2-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

3. <span data-ttu-id="193f2-220">A hello **felhasználókezelés** kattintson **engedélyezését**.</span><span class="sxs-lookup"><span data-stu-id="193f2-220">In hello **User Management** section, click **Requesters**.</span></span>
   
    <span data-ttu-id="193f2-221">![Engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790818.png "engedélyezését")</span><span class="sxs-lookup"><span data-stu-id="193f2-221">![Requesters](./media/active-directory-saas-freshservice-tutorial/ic790818.png "Requesters")</span></span>

4. <span data-ttu-id="193f2-222">Kattintson a **új kérelmező**.</span><span class="sxs-lookup"><span data-stu-id="193f2-222">Click **New Requester**.</span></span>
   
    <span data-ttu-id="193f2-223">![Új engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790819.png "új engedélyezését")</span><span class="sxs-lookup"><span data-stu-id="193f2-223">![New Requesters](./media/active-directory-saas-freshservice-tutorial/ic790819.png "New Requesters")</span></span>

5. <span data-ttu-id="193f2-224">A hello **új kérelmező** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="193f2-224">In hello **New Requester** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="193f2-225">![Új kérelmező](./media/active-directory-saas-freshservice-tutorial/ic790820.png "új kérelmező")</span><span class="sxs-lookup"><span data-stu-id="193f2-225">![New Requester](./media/active-directory-saas-freshservice-tutorial/ic790820.png "New Requester")</span></span>   

    <span data-ttu-id="193f2-226">a.</span><span class="sxs-lookup"><span data-stu-id="193f2-226">a.</span></span> <span data-ttu-id="193f2-227">Adja meg a hello **Utónév** és **E-mail** tooprovision hello a kívánt fiók érvényes Azure Active Directory attribútumok kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="193f2-227">Enter hello **First Name** and **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="193f2-228">b.</span><span class="sxs-lookup"><span data-stu-id="193f2-228">b.</span></span> <span data-ttu-id="193f2-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="193f2-229">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="193f2-230">hello Azure Active Directory fióktulajdonos kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik</span><span class="sxs-lookup"><span data-stu-id="193f2-230">hello Azure Active Directory account holder gets an email including a link tooconfirm hello account before it becomes active</span></span>
    >  

>[!NOTE]
><span data-ttu-id="193f2-231">Bármely más FreshService felhasználói fiók létrehozása eszközök vagy FreshService tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="193f2-231">You can use any other FreshService user account creation tools or APIs provided by FreshService tooprovision AAD user accounts.</span></span>
>  

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="193f2-233">**tooassign Britta Simon tooFreshservice, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="193f2-233">**tooassign Britta Simon tooFreshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="193f2-234">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="193f2-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="193f2-236">Hello alkalmazások listában válassza ki a **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="193f2-236">In hello applications list, select **Freshservice**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. <span data-ttu-id="193f2-238">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="193f2-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="193f2-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="193f2-240">Click **Add** button.</span></span> <span data-ttu-id="193f2-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="193f2-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="193f2-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="193f2-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="193f2-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="193f2-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="193f2-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="193f2-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="193f2-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="193f2-246">Testing single sign-on</span></span>

<span data-ttu-id="193f2-247">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="193f2-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="193f2-248">Ha a hozzáférési Panel hello hello Freshservice csempe gombra kattint, automatikusan bejelentkezett tooyour Freshservice alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="193f2-248">When you click hello Freshservice tile in hello Access Panel, you should get automatically signed-on tooyour Freshservice application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="193f2-249">További források</span><span class="sxs-lookup"><span data-stu-id="193f2-249">Additional resources</span></span>

* [<span data-ttu-id="193f2-250">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="193f2-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="193f2-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="193f2-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

