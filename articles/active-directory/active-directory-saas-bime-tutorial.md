---
title: "Oktatóanyag: Azure Active Directoryval integrált Bime |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Bime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1213725028dd8ce90f22fa6e9c50ffabebc8f3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a><span data-ttu-id="e5c53-103">Oktatóanyag: Azure Active Directoryval integrált Bime</span><span class="sxs-lookup"><span data-stu-id="e5c53-103">Tutorial: Azure Active Directory integration with Bime</span></span>

<span data-ttu-id="e5c53-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Bime az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5c53-104">In this tutorial, you learn how toointegrate Bime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5c53-105">Bime integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-105">Integrating Bime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e5c53-106">Megadhatja a hozzáférés tooBime rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e5c53-106">You can control in Azure AD who has access tooBime</span></span>
- <span data-ttu-id="e5c53-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBime (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e5c53-107">You can enable your users tooautomatically get signed-on tooBime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5c53-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5c53-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e5c53-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5c53-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5c53-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5c53-110">Prerequisites</span></span>

<span data-ttu-id="e5c53-111">az Azure AD integrálása Bime tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-111">tooconfigure Azure AD integration with Bime, you need hello following items:</span></span>

- <span data-ttu-id="e5c53-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e5c53-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5c53-113">Egy Bime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e5c53-113">A Bime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5c53-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e5c53-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5c53-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e5c53-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5c53-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e5c53-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5c53-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5c53-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5c53-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e5c53-118">Scenario description</span></span>
<span data-ttu-id="e5c53-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e5c53-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5c53-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e5c53-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5c53-121">Hello gyűjteményből Bime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5c53-121">Adding Bime from hello gallery</span></span>
2. <span data-ttu-id="e5c53-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5c53-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bime-from-hello-gallery"></a><span data-ttu-id="e5c53-123">Hello gyűjteményből Bime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5c53-123">Adding Bime from hello gallery</span></span>
<span data-ttu-id="e5c53-124">tooconfigure hello integrációja Bime az Azure AD-be, meg kell tooadd Bime hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e5c53-124">tooconfigure hello integration of Bime into Azure AD, you need tooadd Bime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e5c53-125">**tooadd Bime hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5c53-125">**tooadd Bime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5c53-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5c53-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e5c53-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e5c53-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e5c53-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e5c53-133">Hello keresési mezőbe, írja be a **Bime**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-133">In hello search box, type **Bime**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. <span data-ttu-id="e5c53-135">A hello eredmények panelen válassza ki a **Bime**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-135">In hello results panel, select **Bime**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e5c53-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5c53-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e5c53-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Bime.</span><span class="sxs-lookup"><span data-stu-id="e5c53-138">In this section, you configure and test Azure AD single sign-on with Bime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e5c53-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Bime tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e5c53-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bime is tooa user in Azure AD.</span></span> <span data-ttu-id="e5c53-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bime közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e5c53-140">In other words, a link relationship between an Azure AD user and hello related user in Bime needs toobe established.</span></span>

<span data-ttu-id="e5c53-141">Bime, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e5c53-141">In Bime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e5c53-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Bime-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e5c53-142">tooconfigure and test Azure AD single sign-on with Bime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e5c53-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e5c53-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e5c53-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5c53-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5c53-145">**[Bime tesztfelhasználó létrehozása](#creating-a-bime-test-user)**  -toohave egy megfelelője a Britta Simon a Bime, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e5c53-145">**[Creating a Bime test user](#creating-a-bime-test-user)** - toohave a counterpart of Britta Simon in Bime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5c53-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5c53-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5c53-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e5c53-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e5c53-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5c53-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e5c53-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Bime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e5c53-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bime application.</span></span>

<span data-ttu-id="e5c53-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Bime, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5c53-150">**tooconfigure Azure AD single sign-on with Bime, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5c53-151">Az Azure portál, a hello hello **Bime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-151">In hello Azure portal, on hello **Bime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e5c53-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5c53-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. <span data-ttu-id="e5c53-155">A hello **Bime tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-155">On hello **Bime Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    <span data-ttu-id="e5c53-157">a.</span><span class="sxs-lookup"><span data-stu-id="e5c53-157">a.</span></span> <span data-ttu-id="e5c53-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="e5c53-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    <span data-ttu-id="e5c53-159">b.</span><span class="sxs-lookup"><span data-stu-id="e5c53-159">b.</span></span> <span data-ttu-id="e5c53-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="e5c53-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5c53-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e5c53-161">These values are not real.</span></span> <span data-ttu-id="e5c53-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="e5c53-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e5c53-163">Ügyfél [Bime ügyfél-támogatási csoport](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e5c53-163">Contact [Bime Client support team](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget these values.</span></span> 
 
4. <span data-ttu-id="e5c53-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="e5c53-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. <span data-ttu-id="e5c53-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e5c53-168">A hello **Bime konfigurációs** kattintson **konfigurálása Bime** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e5c53-168">On hello **Bime Configuration** section, click **Configure Bime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e5c53-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e5c53-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. <span data-ttu-id="e5c53-171">Egy másik webes böngészőablakban jelentkezzen be a Bime vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e5c53-171">In a different web browser window, log into your Bime company site as an administrator.</span></span>

8. <span data-ttu-id="e5c53-172">Hello eszköztárában kattintson **Admin**, majd **fiók**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-172">In hello toolbar, click **Admin**, and then **Account**.</span></span>
   
    <span data-ttu-id="e5c53-173">![Felügyeleti](./media/active-directory-saas-bime-tutorial/ic775558.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="e5c53-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span></span>

9. <span data-ttu-id="e5c53-174">Hello fiók konfigurációs lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-174">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="e5c53-175">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/ic775559.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e5c53-175">![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/ic775559.png "Configure Single Sign-On")</span></span>
   
    <span data-ttu-id="e5c53-176">a.</span><span class="sxs-lookup"><span data-stu-id="e5c53-176">a.</span></span> <span data-ttu-id="e5c53-177">Válassza ki **engedélyezése SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-177">Select **Enable SAML authentication**.</span></span>

    <span data-ttu-id="e5c53-178">b.</span><span class="sxs-lookup"><span data-stu-id="e5c53-178">b.</span></span> <span data-ttu-id="e5c53-179">A hello **távoli bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e5c53-179">In hello **Remote Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e5c53-180">c.</span><span class="sxs-lookup"><span data-stu-id="e5c53-180">c.</span></span>  <span data-ttu-id="e5c53-181">Beillesztés hello **ujjlenyomat** hello az Azure portálról érték **tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e5c53-181">Paste hello **Thumbprint** value from Azure portal into hello **Certificate Fingerprint** textbox.</span></span>       
   
    <span data-ttu-id="e5c53-182">d.</span><span class="sxs-lookup"><span data-stu-id="e5c53-182">d.</span></span> <span data-ttu-id="e5c53-183">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e5c53-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e5c53-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e5c53-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e5c53-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e5c53-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5c53-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e5c53-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5c53-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="e5c53-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e5c53-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e5c53-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e5c53-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5c53-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5c53-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5c53-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5c53-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5c53-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5c53-199">a.</span><span class="sxs-lookup"><span data-stu-id="e5c53-199">a.</span></span> <span data-ttu-id="e5c53-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5c53-201">b.</span><span class="sxs-lookup"><span data-stu-id="e5c53-201">b.</span></span> <span data-ttu-id="e5c53-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5c53-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5c53-203">c.</span><span class="sxs-lookup"><span data-stu-id="e5c53-203">c.</span></span> <span data-ttu-id="e5c53-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e5c53-205">d.</span><span class="sxs-lookup"><span data-stu-id="e5c53-205">d.</span></span> <span data-ttu-id="e5c53-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-206">Click **Create**.</span></span>
 
### <a name="creating-a-bime-test-user"></a><span data-ttu-id="e5c53-207">Bime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5c53-207">Creating a Bime test user</span></span>

<span data-ttu-id="e5c53-208">A sorrend tooenable az Azure AD felhasználók toolog a tooBime azok ki kell építenie Bime be.</span><span class="sxs-lookup"><span data-stu-id="e5c53-208">In order tooenable Azure AD users toolog in tooBime, they must be provisioned into Bime.</span></span> <span data-ttu-id="e5c53-209">Bime hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e5c53-209">In hello case of Bime, provisioning is a manual task.</span></span>

<span data-ttu-id="e5c53-210">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5c53-210">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5c53-211">Jelentkezzen be tooyour **Bime** bérlő.</span><span class="sxs-lookup"><span data-stu-id="e5c53-211">Log in tooyour **Bime** tenant.</span></span>

2. <span data-ttu-id="e5c53-212">Hello eszköztárában kattintson **Admin**, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-212">In hello toolbar, click **Admin**, and then **Users**.</span></span>
   
    <span data-ttu-id="e5c53-213">![Felügyeleti](./media/active-directory-saas-bime-tutorial/ic775561.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="e5c53-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span></span>

3. <span data-ttu-id="e5c53-214">A hello **felhasználók listában**, kattintson a **új felhasználó hozzáadása** ("+").</span><span class="sxs-lookup"><span data-stu-id="e5c53-214">In hello **Users List**, click **Add New User** (“+”).</span></span>
   
    <span data-ttu-id="e5c53-215">![Felhasználók](./media/active-directory-saas-bime-tutorial/ic775562.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="e5c53-215">![Users](./media/active-directory-saas-bime-tutorial/ic775562.png "Users")</span></span>

4. <span data-ttu-id="e5c53-216">A hello **felhasználó adatait** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5c53-216">On hello **User Details** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="e5c53-217">![Felhasználó adatait](./media/active-directory-saas-bime-tutorial/ic775563.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="e5c53-217">![User Details](./media/active-directory-saas-bime-tutorial/ic775563.png "User Details")</span></span>
   
    <span data-ttu-id="e5c53-218">a.</span><span class="sxs-lookup"><span data-stu-id="e5c53-218">a.</span></span> <span data-ttu-id="e5c53-219">A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-219">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="e5c53-220">b.</span><span class="sxs-lookup"><span data-stu-id="e5c53-220">b.</span></span> <span data-ttu-id="e5c53-221">A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-221">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="e5c53-222">c.</span><span class="sxs-lookup"><span data-stu-id="e5c53-222">c.</span></span> <span data-ttu-id="e5c53-223">A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="e5c53-223">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="e5c53-224">d.</span><span class="sxs-lookup"><span data-stu-id="e5c53-224">d.</span></span> <span data-ttu-id="e5c53-225">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-225">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="e5c53-226">Bármely más Bime felhasználói fiók létrehozása eszközök vagy Bime tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e5c53-226">You can use any other Bime user account creation tools or APIs provided by Bime tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e5c53-227">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e5c53-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e5c53-228">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBime megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e5c53-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBime.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e5c53-230">**tooassign Britta Simon tooBime, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e5c53-230">**tooassign Britta Simon tooBime, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5c53-231">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e5c53-233">Hello alkalmazások listában válassza ki a **Bime**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-233">In hello applications list, select **Bime**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. <span data-ttu-id="e5c53-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e5c53-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e5c53-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5c53-237">Click **Add** button.</span></span> <span data-ttu-id="e5c53-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5c53-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e5c53-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e5c53-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e5c53-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5c53-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5c53-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5c53-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e5c53-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e5c53-243">Testing single sign-on</span></span>

<span data-ttu-id="e5c53-244">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="e5c53-244">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e5c53-245">Ha a hozzáférési Panel hello hello Bime csempe gombra kattint, automatikusan bejelentkezett tooyour Bime alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e5c53-245">When you click hello Bime tile in hello Access Panel, you should get automatically signed-on tooyour Bime application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5c53-246">További források</span><span class="sxs-lookup"><span data-stu-id="e5c53-246">Additional resources</span></span>

* [<span data-ttu-id="e5c53-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e5c53-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5c53-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e5c53-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

