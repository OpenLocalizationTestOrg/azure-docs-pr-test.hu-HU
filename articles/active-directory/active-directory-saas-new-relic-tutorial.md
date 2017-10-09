---
title: "Oktatóanyag: Azure Active Directory-integráció a New Relic |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a New Relic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: dc8f0df3b18a32bde155e8911a581fc5f91af217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="433fa-103">Oktatóanyag: A New Relic Azure Active Directory-integráció</span><span class="sxs-lookup"><span data-stu-id="433fa-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="433fa-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate New Relic az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="433fa-104">In this tutorial, you learn how toointegrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="433fa-105">New Relic integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="433fa-105">Integrating New Relic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="433fa-106">Megadhatja a hozzáférés tooNew Relic rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="433fa-106">You can control in Azure AD who has access tooNew Relic</span></span>
- <span data-ttu-id="433fa-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNew Relic (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="433fa-107">You can enable your users tooautomatically get signed-on tooNew Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="433fa-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="433fa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="433fa-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="433fa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="433fa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="433fa-110">Prerequisites</span></span>

<span data-ttu-id="433fa-111">az Azure AD-integráció a New Relic tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="433fa-111">tooconfigure Azure AD integration with New Relic, you need hello following items:</span></span>

- <span data-ttu-id="433fa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="433fa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="433fa-113">Egy új New Relic egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="433fa-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="433fa-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="433fa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="433fa-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="433fa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="433fa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="433fa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="433fa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="433fa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="433fa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="433fa-118">Scenario description</span></span>
<span data-ttu-id="433fa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="433fa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="433fa-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="433fa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="433fa-121">New Relic hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="433fa-121">Adding New Relic from hello gallery</span></span>
2. <span data-ttu-id="433fa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="433fa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-hello-gallery"></a><span data-ttu-id="433fa-123">New Relic hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="433fa-123">Adding New Relic from hello gallery</span></span>
<span data-ttu-id="433fa-124">tooconfigure hello integrációja New Relic az Azure AD-be, meg kell tooadd New Relic hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="433fa-124">tooconfigure hello integration of New Relic into Azure AD, you need tooadd New Relic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="433fa-125">**tooadd New Relic hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="433fa-125">**tooadd New Relic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="433fa-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="433fa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="433fa-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="433fa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="433fa-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="433fa-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="433fa-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="433fa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="433fa-133">Hello keresési mezőbe, írja be a **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="433fa-133">In hello search box, type **New Relic**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="433fa-135">A hello eredmények panelen válassza ki a **New Relic**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="433fa-135">In hello results panel, select **New Relic**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="433fa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="433fa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="433fa-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló új New Relic.</span><span class="sxs-lookup"><span data-stu-id="433fa-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="433fa-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a New Relic tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="433fa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in New Relic is tooa user in Azure AD.</span></span> <span data-ttu-id="433fa-140">Ez azt jelenti hello kapcsolódó felhasználó a New Relic és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="433fa-140">In other words, a link relationship between an Azure AD user and hello related user in New Relic needs toobe established.</span></span>

<span data-ttu-id="433fa-141">A New Relic rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="433fa-141">In New Relic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="433fa-142">tooconfigure és az Azure AD az egyszeri bejelentkezés New Relic-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="433fa-142">tooconfigure and test Azure AD single sign-on with New Relic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="433fa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="433fa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="433fa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="433fa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="433fa-145">**[New Relic tesztfelhasználó létrehozása](#creating-a-new-relic-test-user)**  -toohave egy megfelelője a Britta Simon a New Relic, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="433fa-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - toohave a counterpart of Britta Simon in New Relic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="433fa-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="433fa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="433fa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="433fa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="433fa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="433fa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="433fa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a New Relic-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="433fa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="433fa-150">**az Azure AD tooconfigure egyszeri bejelentkezést a New Relic, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="433fa-150">**tooconfigure Azure AD single sign-on with New Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="433fa-151">Az Azure portál, a hello hello **New Relic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="433fa-151">In hello Azure portal, on hello **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="433fa-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="433fa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="433fa-155">A hello **új New Relic-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="433fa-155">On hello **New Relic Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="433fa-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="433fa-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="433fa-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="433fa-158">hello value is not real.</span></span> <span data-ttu-id="433fa-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="433fa-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="433fa-160">Ügyfél [új New Relic-ügyfél-támogatási csoport](https://support.newrelic.com/) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="433fa-160">Contact [New Relic Client support team](https://support.newrelic.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="433fa-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="433fa-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="433fa-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="433fa-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="433fa-165">A hello **új New Relic-konfiguráció** kattintson **New Relic konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="433fa-165">On hello **New Relic Configuration** section, click **Configure New Relic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="433fa-166">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="433fa-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="433fa-168">Egy másik webes böngészőablakban tooyour bejelentkezés **New Relic** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="433fa-168">In a different web browser window, sign on tooyour **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="433fa-169">Hello hello felső menüben kattintson a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="433fa-169">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="433fa-170">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="433fa-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="433fa-171">Kattintson a hello **biztonsági és hitelesítési** fülre, majd hello **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="433fa-171">Click hello **Security and authentication** tab, and then click hello **Single sign on** tab.</span></span>
   
    <span data-ttu-id="433fa-172">![Egyszeri bejelentkezés](./media/active-directory-saas-new-relic-tutorial/ic797037.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="433fa-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="433fa-173">Hello SAML párbeszédpanel lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="433fa-173">On hello SAML dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="433fa-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="433fa-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="433fa-175">a.</span><span class="sxs-lookup"><span data-stu-id="433fa-175">a.</span></span> <span data-ttu-id="433fa-176">Kattintson a **Choose File** tooupload a letöltött Azure Active Directory-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="433fa-176">Click **Choose File** tooupload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="433fa-177">b.</span><span class="sxs-lookup"><span data-stu-id="433fa-177">b.</span></span> <span data-ttu-id="433fa-178">A hello **távoli bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="433fa-178">In hello **Remote login URL** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="433fa-179">c.</span><span class="sxs-lookup"><span data-stu-id="433fa-179">c.</span></span> <span data-ttu-id="433fa-180">A hello **kijelentkezési URL-cím üzenetsorokra** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="433fa-180">In hello **Logout landing URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="433fa-181">d.</span><span class="sxs-lookup"><span data-stu-id="433fa-181">d.</span></span> <span data-ttu-id="433fa-182">Kattintson a **menti a módosításokat**.</span><span class="sxs-lookup"><span data-stu-id="433fa-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="433fa-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="433fa-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="433fa-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="433fa-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="433fa-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="433fa-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="433fa-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="433fa-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="433fa-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="433fa-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="433fa-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="433fa-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="433fa-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="433fa-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="433fa-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="433fa-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="433fa-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="433fa-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="433fa-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="433fa-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="433fa-198">a.</span><span class="sxs-lookup"><span data-stu-id="433fa-198">a.</span></span> <span data-ttu-id="433fa-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="433fa-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="433fa-200">b.</span><span class="sxs-lookup"><span data-stu-id="433fa-200">b.</span></span> <span data-ttu-id="433fa-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="433fa-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="433fa-202">c.</span><span class="sxs-lookup"><span data-stu-id="433fa-202">c.</span></span> <span data-ttu-id="433fa-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="433fa-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="433fa-204">d.</span><span class="sxs-lookup"><span data-stu-id="433fa-204">d.</span></span> <span data-ttu-id="433fa-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="433fa-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="433fa-206">New Relic tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="433fa-206">Creating a New Relic test user</span></span>

<span data-ttu-id="433fa-207">A sorrend tooenable Azure Active Directory felhasználók toolog a New Relic tooNew azok ki kell építenie a New Relic.</span><span class="sxs-lookup"><span data-stu-id="433fa-207">In order tooenable Azure Active Directory users toolog in tooNew Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="433fa-208">New Relic hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="433fa-208">In hello case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="433fa-209">**tooprovision egy felhasználói fiók tooNew Relic, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="433fa-209">**tooprovision a user account tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="433fa-210">Jelentkezzen be tooyour **New Relic** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="433fa-210">Log in tooyour **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="433fa-211">Hello hello felső menüben kattintson a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="433fa-211">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="433fa-212">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="433fa-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="433fa-213">A hello **fiók** hello ablaktábla bal oldalán, kattintson a **összegzés**, és kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="433fa-213">In hello **Account** pane on hello left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="433fa-214">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="433fa-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="433fa-215">A hello **aktív felhasználók** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="433fa-215">On hello **Active users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="433fa-216">![Aktív felhasználók](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktív felhasználók")</span><span class="sxs-lookup"><span data-stu-id="433fa-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="433fa-217">a.</span><span class="sxs-lookup"><span data-stu-id="433fa-217">a.</span></span> <span data-ttu-id="433fa-218">A hello **E-mail** szövegmezőhöz típus hello e-mail cím egy érvényes Azure Active Directory-felhasználó kívánt tooprovision.</span><span class="sxs-lookup"><span data-stu-id="433fa-218">In hello **Email** textbox, type hello email address of a valid Azure Active Directory user you want tooprovision.</span></span>

    <span data-ttu-id="433fa-219">b.</span><span class="sxs-lookup"><span data-stu-id="433fa-219">b.</span></span> <span data-ttu-id="433fa-220">Mint **szerepkör** válasszon **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="433fa-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="433fa-221">c.</span><span class="sxs-lookup"><span data-stu-id="433fa-221">c.</span></span> <span data-ttu-id="433fa-222">Kattintson a **adja hozzá a felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="433fa-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="433fa-223">Bármely más New Relic felhasználói fiók létrehozása eszközök, vagy új New Relic tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="433fa-223">You can use any other New Relic user account creation tools or APIs provided by New Relic tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="433fa-224">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="433fa-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="433fa-225">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNew Relic megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="433fa-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNew Relic.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="433fa-227">**tooassign Britta Simon tooNew Relic, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="433fa-227">**tooassign Britta Simon tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="433fa-228">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="433fa-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="433fa-230">Hello alkalmazások listában válassza ki a **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="433fa-230">In hello applications list, select **New Relic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="433fa-232">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="433fa-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="433fa-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="433fa-234">Click **Add** button.</span></span> <span data-ttu-id="433fa-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="433fa-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="433fa-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="433fa-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="433fa-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="433fa-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="433fa-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="433fa-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="433fa-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="433fa-240">Testing single sign-on</span></span>

<span data-ttu-id="433fa-241">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="433fa-241">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="433fa-242">Hello New Relic csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour New Relic alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="433fa-242">When you click hello New Relic tile in hello Access Panel, you should get automatically signed-on tooyour New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="433fa-243">További források</span><span class="sxs-lookup"><span data-stu-id="433fa-243">Additional resources</span></span>

* [<span data-ttu-id="433fa-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="433fa-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="433fa-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="433fa-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

