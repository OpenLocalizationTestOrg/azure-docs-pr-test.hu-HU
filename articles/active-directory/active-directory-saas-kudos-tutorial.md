---
title: "Oktatóanyag: Azure Active Directoryval integrált Kudos |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Kudos között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: c1b481463574461f9948db2b83843fefa5d74e99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="c65a8-103">Oktatóanyag: Azure Active Directoryval integrált Kudos</span><span class="sxs-lookup"><span data-stu-id="c65a8-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="c65a8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kudos az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c65a8-104">In this tutorial, you learn how toointegrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c65a8-105">Kudos integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-105">Integrating Kudos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c65a8-106">Megadhatja a hozzáférés tooKudos rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c65a8-106">You can control in Azure AD who has access tooKudos</span></span>
- <span data-ttu-id="c65a8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKudos (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c65a8-107">You can enable your users tooautomatically get signed-on tooKudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c65a8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c65a8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c65a8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c65a8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c65a8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c65a8-110">Prerequisites</span></span>

<span data-ttu-id="c65a8-111">az Azure AD integrálása Kudos tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-111">tooconfigure Azure AD integration with Kudos, you need hello following items:</span></span>

- <span data-ttu-id="c65a8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c65a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c65a8-113">Egy Kudos egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c65a8-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c65a8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c65a8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c65a8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c65a8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c65a8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c65a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c65a8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c65a8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c65a8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c65a8-118">Scenario description</span></span>
<span data-ttu-id="c65a8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c65a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c65a8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c65a8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c65a8-121">Hello gyűjteményből Kudos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c65a8-121">Adding Kudos from hello gallery</span></span>
2. <span data-ttu-id="c65a8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c65a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-hello-gallery"></a><span data-ttu-id="c65a8-123">Hello gyűjteményből Kudos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c65a8-123">Adding Kudos from hello gallery</span></span>
<span data-ttu-id="c65a8-124">tooconfigure hello integrációja Kudos az Azure AD-be, meg kell tooadd Kudos hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c65a8-124">tooconfigure hello integration of Kudos into Azure AD, you need tooadd Kudos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c65a8-125">**tooadd Kudos hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c65a8-125">**tooadd Kudos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65a8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c65a8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c65a8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c65a8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c65a8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c65a8-133">Hello keresési mezőbe, írja be a **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-133">In hello search box, type **Kudos**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="c65a8-135">A hello eredmények panelen válassza ki a **Kudos**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-135">In hello results panel, select **Kudos**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c65a8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c65a8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c65a8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kudos.</span><span class="sxs-lookup"><span data-stu-id="c65a8-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c65a8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kudos tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c65a8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kudos is tooa user in Azure AD.</span></span> <span data-ttu-id="c65a8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Kudos közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c65a8-140">In other words, a link relationship between an Azure AD user and hello related user in Kudos needs toobe established.</span></span>

<span data-ttu-id="c65a8-141">Kudos, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c65a8-141">In Kudos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c65a8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Kudos-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c65a8-142">tooconfigure and test Azure AD single sign-on with Kudos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c65a8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c65a8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c65a8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c65a8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c65a8-145">**[Kudos tesztfelhasználó létrehozása](#creating-a-kudos-test-user)**  -toohave egy megfelelője a Britta Simon a Kudos, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c65a8-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - toohave a counterpart of Britta Simon in Kudos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c65a8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c65a8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c65a8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c65a8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c65a8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c65a8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c65a8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Kudos alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c65a8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="c65a8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Kudos, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c65a8-150">**tooconfigure Azure AD single sign-on with Kudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65a8-151">Az Azure portál, a hello hello **Kudos** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-151">In hello Azure portal, on hello **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c65a8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c65a8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="c65a8-155">A hello **Kudos tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-155">On hello **Kudos Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="c65a8-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="c65a8-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c65a8-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c65a8-158">This value is not real.</span></span> <span data-ttu-id="c65a8-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c65a8-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c65a8-160">Ügyfél [Kudos ügyfél-támogatási csoport](http://success.kudosnow.com/home) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c65a8-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) tooget this value.</span></span> 
 
4. <span data-ttu-id="c65a8-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c65a8-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="c65a8-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c65a8-165">A hello **Kudos konfigurációs** kattintson **konfigurálása Kudos** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c65a8-165">On hello **Kudos Configuration** section, click **Configure Kudos** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c65a8-166">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c65a8-166">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="c65a8-168">Egy másik webes böngészőablakban jelentkezzen be a Kudos vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c65a8-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="c65a8-169">Hello hello felső menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-169">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="c65a8-170">![Beállítások](./media/active-directory-saas-kudos-tutorial/ic787806.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="c65a8-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="c65a8-171">Kattintson a **integrációja \> SSO**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="c65a8-172">A hello **SSO** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-172">In hello **SSO** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="c65a8-173">![EGYSZERI BEJELENTKEZÉS](./media/active-directory-saas-kudos-tutorial/ic787807.png "EGYSZERI BEJELENTKEZÉS")</span><span class="sxs-lookup"><span data-stu-id="c65a8-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="c65a8-174">a.</span><span class="sxs-lookup"><span data-stu-id="c65a8-174">a.</span></span> <span data-ttu-id="c65a8-175">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c65a8-175">In **Sign on URL** textbox, paste hello value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="c65a8-176">b.</span><span class="sxs-lookup"><span data-stu-id="c65a8-176">b.</span></span> <span data-ttu-id="c65a8-177">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="c65a8-177">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="c65a8-178">c.</span><span class="sxs-lookup"><span data-stu-id="c65a8-178">c.</span></span> <span data-ttu-id="c65a8-179">A **kijelentkezési tooURL**, illessze be a hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c65a8-179">In **Logout tooURL**, paste hello value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="c65a8-180">d.</span><span class="sxs-lookup"><span data-stu-id="c65a8-180">d.</span></span> <span data-ttu-id="c65a8-181">A hello **a Kudos URL-cím** szövegmező, írja be a cég nevét.</span><span class="sxs-lookup"><span data-stu-id="c65a8-181">In hello **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="c65a8-182">e.</span><span class="sxs-lookup"><span data-stu-id="c65a8-182">e.</span></span> <span data-ttu-id="c65a8-183">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c65a8-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c65a8-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c65a8-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c65a8-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c65a8-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c65a8-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c65a8-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c65a8-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="c65a8-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c65a8-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c65a8-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c65a8-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65a8-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c65a8-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c65a8-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c65a8-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c65a8-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c65a8-199">a.</span><span class="sxs-lookup"><span data-stu-id="c65a8-199">a.</span></span> <span data-ttu-id="c65a8-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c65a8-201">b.</span><span class="sxs-lookup"><span data-stu-id="c65a8-201">b.</span></span> <span data-ttu-id="c65a8-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c65a8-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c65a8-203">c.</span><span class="sxs-lookup"><span data-stu-id="c65a8-203">c.</span></span> <span data-ttu-id="c65a8-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c65a8-205">d.</span><span class="sxs-lookup"><span data-stu-id="c65a8-205">d.</span></span> <span data-ttu-id="c65a8-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="c65a8-207">Kudos tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c65a8-207">Creating a Kudos test user</span></span>

<span data-ttu-id="c65a8-208">A sorrend tooenable az Azure AD felhasználók toolog Kudos be azok ki kell építenie Kudos be.</span><span class="sxs-lookup"><span data-stu-id="c65a8-208">In order tooenable Azure AD users toolog into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="c65a8-209">Kudos hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c65a8-209">In hello case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="c65a8-210">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c65a8-210">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65a8-211">Jelentkezzen be tooyour **Kudos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c65a8-211">Log in tooyour **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="c65a8-212">Hello hello felső menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-212">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="c65a8-213">![Beállítások](./media/active-directory-saas-kudos-tutorial/ic787806.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="c65a8-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="c65a8-214">Kattintson a **felhasználói Admin**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="c65a8-215">Kattintson a hello **felhasználók** fülre, majd **hozzáadni egy felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-215">Click hello **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="c65a8-216">![Felhasználó rendszergazda](./media/active-directory-saas-kudos-tutorial/ic787809.png "felhasználó rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="c65a8-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="c65a8-217">A hello **hozzáadni egy felhasználót** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c65a8-217">In hello **Add a User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="c65a8-218">![Felhasználó hozzáadása](./media/active-directory-saas-kudos-tutorial/ic787810.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="c65a8-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="c65a8-219">a.</span><span class="sxs-lookup"><span data-stu-id="c65a8-219">a.</span></span> <span data-ttu-id="c65a8-220">Típus hello **Utónév**, **Vezetéknév**, **E-mail** és egyéb részleteket a egy érvényes Azure Active Directory-fiók tooprovision a kívánt hello kapcsolatos szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="c65a8-220">Type hello **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="c65a8-221">b.</span><span class="sxs-lookup"><span data-stu-id="c65a8-221">b.</span></span> <span data-ttu-id="c65a8-222">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="c65a8-223">Bármely más Kudos felhasználói fiók létrehozása eszközök vagy Kudos tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c65a8-223">You can use any other Kudos user account creation tools or APIs provided by Kudos tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c65a8-224">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c65a8-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c65a8-225">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKudos megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c65a8-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKudos.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c65a8-227">**tooassign Britta Simon tooKudos, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c65a8-227">**tooassign Britta Simon tooKudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="c65a8-228">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c65a8-230">Hello alkalmazások listában válassza ki a **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-230">In hello applications list, select **Kudos**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="c65a8-232">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c65a8-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c65a8-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c65a8-234">Click **Add** button.</span></span> <span data-ttu-id="c65a8-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c65a8-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c65a8-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c65a8-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c65a8-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c65a8-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c65a8-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c65a8-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c65a8-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c65a8-240">Testing single sign-on</span></span>

<span data-ttu-id="c65a8-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c65a8-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c65a8-242">Ha a hozzáférési Panel hello hello Kudos csempe gombra kattint, automatikusan bejelentkezett tooyour Kudos alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c65a8-242">When you click hello Kudos tile in hello Access Panel, you should get automatically signed-on tooyour Kudos application.</span></span> <span data-ttu-id="c65a8-243">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c65a8-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c65a8-244">További források</span><span class="sxs-lookup"><span data-stu-id="c65a8-244">Additional resources</span></span>

* [<span data-ttu-id="c65a8-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c65a8-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c65a8-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c65a8-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

