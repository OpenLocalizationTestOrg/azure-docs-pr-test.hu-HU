---
title: "Oktatóanyag: Azure Active Directoryval integrált ThousandEyes |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ThousandEyes között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: fbfbfb71809355b1b138762757a851907737730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a><span data-ttu-id="92000-103">Oktatóanyag: Azure Active Directoryval integrált ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="92000-103">Tutorial: Azure Active Directory integration with ThousandEyes</span></span>

<span data-ttu-id="92000-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ThousandEyes az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="92000-104">In this tutorial, you learn how toointegrate ThousandEyes with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="92000-105">ThousandEyes integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="92000-105">Integrating ThousandEyes with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="92000-106">Megadhatja a hozzáférés tooThousandEyes rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="92000-106">You can control in Azure AD who has access tooThousandEyes</span></span>
- <span data-ttu-id="92000-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooThousandEyes (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="92000-107">You can enable your users tooautomatically get signed-on tooThousandEyes (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="92000-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="92000-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="92000-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="92000-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92000-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92000-110">Prerequisites</span></span>

<span data-ttu-id="92000-111">az Azure AD integrálása ThousandEyes tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="92000-111">tooconfigure Azure AD integration with ThousandEyes, you need hello following items:</span></span>

- <span data-ttu-id="92000-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="92000-112">An Azure AD subscription</span></span>
- <span data-ttu-id="92000-113">Egy ThousandEyes egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="92000-113">A ThousandEyes single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="92000-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="92000-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="92000-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="92000-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="92000-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="92000-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="92000-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="92000-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="92000-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="92000-118">Scenario description</span></span>
<span data-ttu-id="92000-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="92000-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="92000-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="92000-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="92000-121">Hello gyűjteményből ThousandEyes hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92000-121">Adding ThousandEyes from hello gallery</span></span>
2. <span data-ttu-id="92000-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="92000-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thousandeyes-from-hello-gallery"></a><span data-ttu-id="92000-123">Hello gyűjteményből ThousandEyes hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92000-123">Adding ThousandEyes from hello gallery</span></span>
<span data-ttu-id="92000-124">tooconfigure hello integrációja ThousandEyes az Azure AD-be, meg kell tooadd ThousandEyes hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="92000-124">tooconfigure hello integration of ThousandEyes into Azure AD, you need tooadd ThousandEyes from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="92000-125">**tooadd ThousandEyes hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="92000-125">**tooadd ThousandEyes from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="92000-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="92000-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="92000-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="92000-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="92000-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="92000-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="92000-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="92000-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="92000-133">Hello keresési mezőbe, írja be a **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="92000-133">In hello search box, type **ThousandEyes**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. <span data-ttu-id="92000-135">A hello eredmények panelen válassza ki a **ThousandEyes**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="92000-135">In hello results panel, select **ThousandEyes**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="92000-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="92000-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="92000-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="92000-138">In this section, you configure and test Azure AD single sign-on with ThousandEyes based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="92000-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ThousandEyes tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="92000-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ThousandEyes is tooa user in Azure AD.</span></span> <span data-ttu-id="92000-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ThousandEyes közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="92000-140">In other words, a link relationship between an Azure AD user and hello related user in ThousandEyes needs toobe established.</span></span>

<span data-ttu-id="92000-141">ThousandEyes, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="92000-141">In ThousandEyes, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="92000-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ThousandEyes-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="92000-142">tooconfigure and test Azure AD single sign-on with ThousandEyes, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="92000-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="92000-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="92000-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92000-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="92000-145">**[ThousandEyes tesztfelhasználó létrehozása](#creating-a-thousandeyes-test-user)**  -toohave egy megfelelője a Britta Simon a ThousandEyes, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="92000-145">**[Creating a ThousandEyes test user](#creating-a-thousandeyes-test-user)** - toohave a counterpart of Britta Simon in ThousandEyes that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="92000-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="92000-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="92000-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="92000-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="92000-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="92000-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="92000-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ThousandEyes alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="92000-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ThousandEyes application.</span></span>

<span data-ttu-id="92000-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ThousandEyes, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="92000-150">**tooconfigure Azure AD single sign-on with ThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="92000-151">Az Azure portál, a hello hello **ThousandEyes** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="92000-151">In hello Azure portal, on hello **ThousandEyes** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="92000-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="92000-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. <span data-ttu-id="92000-155">A hello **ThousandEyes tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="92000-155">On hello **ThousandEyes Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    <span data-ttu-id="92000-157">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://app.thousandeyes.com/login/sso`</span><span class="sxs-lookup"><span data-stu-id="92000-157">In hello **Sign-on URL** textbox, type a URL as: `https://app.thousandeyes.com/login/sso`</span></span>

4. <span data-ttu-id="92000-158">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="92000-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. <span data-ttu-id="92000-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="92000-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="92000-162">A hello **ThousandEyes konfigurációs** kattintson **konfigurálása ThousandEyes** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="92000-162">On hello **ThousandEyes Configuration** section, click **Configure ThousandEyes** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="92000-163">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="92000-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. <span data-ttu-id="92000-165">Egy másik webes böngészőablakban tooyour bejelentkezés **ThousandEyes** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="92000-165">In a different web browser window, sign on tooyour **ThousandEyes** company site as an administrator.</span></span>

8. <span data-ttu-id="92000-166">Hello hello felső menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="92000-166">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="92000-167">![Beállítások](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="92000-167">![Settings](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Settings")</span></span>

9. <span data-ttu-id="92000-168">Kattintson a **fiók**</span><span class="sxs-lookup"><span data-stu-id="92000-168">Click **Account**</span></span>
   
    <span data-ttu-id="92000-169">![Fiók](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="92000-169">![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")</span></span>

10. <span data-ttu-id="92000-170">Kattintson a hello **biztonsági és hitelesítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="92000-170">Click hello **Security & Authentication** tab.</span></span>
   
    <span data-ttu-id="92000-171">![Biztonsági és hitelesítési](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "biztonsági és hitelesítési")</span><span class="sxs-lookup"><span data-stu-id="92000-171">![Security & Authentication](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Security & Authentication")</span></span>

11. <span data-ttu-id="92000-172">A hello **telepítő egyszeri bejelentkezés** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="92000-172">In hello **Setup Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="92000-173">![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "egyszeri bejelentkezés beállítása")</span><span class="sxs-lookup"><span data-stu-id="92000-173">![Setup Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Setup Single Sign-On")</span></span>
  
    <span data-ttu-id="92000-174">a.</span><span class="sxs-lookup"><span data-stu-id="92000-174">a.</span></span> <span data-ttu-id="92000-175">Válassza ki **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="92000-175">Select **Enable Single Sign-On**.</span></span>
  
    <span data-ttu-id="92000-176">b.</span><span class="sxs-lookup"><span data-stu-id="92000-176">b.</span></span> <span data-ttu-id="92000-177">A **bejelentkezési lap URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="92000-177">In **Login Page URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="92000-178">c.</span><span class="sxs-lookup"><span data-stu-id="92000-178">c.</span></span> <span data-ttu-id="92000-179">A **kijelentkezési URL-címe** szövegmezőhöz Beillesztés **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="92000-179">In **Logout Page URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="92000-180">d.</span><span class="sxs-lookup"><span data-stu-id="92000-180">d.</span></span> <span data-ttu-id="92000-181">**Identity Provider kibocsátó** szövegmezőhöz Beillesztés **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="92000-181">**Identity Provider Issuer** textbox, paste **SAML Entity ID** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="92000-182">e.</span><span class="sxs-lookup"><span data-stu-id="92000-182">e.</span></span> <span data-ttu-id="92000-183">A **ellenőrző tanúsítvány**, kattintson a **fájl kiválasztása**, majd töltse fel az Azure-portálról letöltött hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="92000-183">In **Verification Certificate**, click **Choose file**, and then upload hello certificate you have downloaded from Azure portal.</span></span>
  
    <span data-ttu-id="92000-184">f.</span><span class="sxs-lookup"><span data-stu-id="92000-184">f.</span></span> <span data-ttu-id="92000-185">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="92000-185">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="92000-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="92000-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="92000-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="92000-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="92000-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="92000-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="92000-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="92000-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="92000-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="92000-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="92000-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="92000-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="92000-193">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="92000-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="92000-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="92000-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="92000-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="92000-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="92000-199">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="92000-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="92000-201">a.</span><span class="sxs-lookup"><span data-stu-id="92000-201">a.</span></span> <span data-ttu-id="92000-202">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="92000-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="92000-203">b.</span><span class="sxs-lookup"><span data-stu-id="92000-203">b.</span></span> <span data-ttu-id="92000-204">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="92000-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="92000-205">c.</span><span class="sxs-lookup"><span data-stu-id="92000-205">c.</span></span> <span data-ttu-id="92000-206">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="92000-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="92000-207">d.</span><span class="sxs-lookup"><span data-stu-id="92000-207">d.</span></span> <span data-ttu-id="92000-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="92000-208">Click **Create**.</span></span>
 
### <a name="creating-a-thousandeyes-test-user"></a><span data-ttu-id="92000-209">ThousandEyes tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="92000-209">Creating a ThousandEyes test user</span></span>

<span data-ttu-id="92000-210">A sorrend tooenable az Azure AD felhasználók toolog ThousandEyes be azok ki kell építenie ThousandEyes be.</span><span class="sxs-lookup"><span data-stu-id="92000-210">In order tooenable Azure AD users toolog into ThousandEyes, they must be provisioned into ThousandEyes.</span></span>  
<span data-ttu-id="92000-211">ThousandEyes hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="92000-211">In hello case of ThousandEyes, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="92000-212">Bármely más ThousandEyes felhasználói fiók létrehozása eszközök vagy ThousandEyes tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="92000-212">You can use any other ThousandEyes user account creation tools or APIs provided by ThousandEyes tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="92000-213">**egy felhasználói fiók tooThousandEyes tooprovision hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="92000-213">**tooprovision a user account tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="92000-214">Jelentkezzen be a ThousandEyes vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="92000-214">Log into your ThousandEyes company site as an administrator.</span></span>

2. <span data-ttu-id="92000-215">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="92000-215">Click **Settings**.</span></span>
   
    <span data-ttu-id="92000-216">![Beállítások](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="92000-216">![Settings](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")</span></span>

3. <span data-ttu-id="92000-217">Kattintson a **fiók**.</span><span class="sxs-lookup"><span data-stu-id="92000-217">Click **Account**.</span></span>
   
    <span data-ttu-id="92000-218">![Fiók](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="92000-218">![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")</span></span>

4. <span data-ttu-id="92000-219">Kattintson a hello **fiókok és a felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="92000-219">Click hello **Accounts & Users** tab.</span></span>
   
    <span data-ttu-id="92000-220">![Fiókok és a felhasználók](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "fiókok és a felhasználók")</span><span class="sxs-lookup"><span data-stu-id="92000-220">![Accounts & Users](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")</span></span>

5. <span data-ttu-id="92000-221">A hello **felhasználók hozzáadása és fiókok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="92000-221">In hello **Add Users & Accounts** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="92000-222">![Felhasználói fiókok hozzáadása](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "felhasználói fiókok hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="92000-222">![Add User Accounts](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")</span></span>   
  
    <span data-ttu-id="92000-223">a.</span><span class="sxs-lookup"><span data-stu-id="92000-223">a.</span></span> <span data-ttu-id="92000-224">A **neve** szövegmező, például a felhasználó hello típusnév **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="92000-224">In **Name** textbox, type hello name of user like **Britta Simon**.</span></span>

    <span data-ttu-id="92000-225">b.</span><span class="sxs-lookup"><span data-stu-id="92000-225">b.</span></span> <span data-ttu-id="92000-226">A **E-mail** szövegmezőhöz: hello e-mail felhasználó például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="92000-226">In **Email** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="92000-227">b.</span><span class="sxs-lookup"><span data-stu-id="92000-227">b.</span></span> <span data-ttu-id="92000-228">Kattintson a **új felhasználó hozzáadása tooAccount**.</span><span class="sxs-lookup"><span data-stu-id="92000-228">Click **Add New User tooAccount**.</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="92000-229">hello Azure Active Directory fióktulajdonos egy hivatkozás tooconfirm például egy e-mailek, és a hello fiók aktiválása.</span><span class="sxs-lookup"><span data-stu-id="92000-229">hello Azure Active Directory account holder will get an email including a link tooconfirm and activate hello account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="92000-230">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="92000-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="92000-231">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooThousandEyes megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="92000-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThousandEyes.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="92000-233">**tooassign Britta Simon tooThousandEyes, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="92000-233">**tooassign Britta Simon tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="92000-234">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="92000-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="92000-236">Hello alkalmazások listában válassza ki a **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="92000-236">In hello applications list, select **ThousandEyes**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. <span data-ttu-id="92000-238">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="92000-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="92000-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="92000-240">Click **Add** button.</span></span> <span data-ttu-id="92000-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="92000-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="92000-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="92000-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="92000-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="92000-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="92000-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="92000-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="92000-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="92000-246">Testing single sign-on</span></span>

<span data-ttu-id="92000-247">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="92000-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="92000-248">Hello ThousandEyes hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ThousandEyes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="92000-248">When you click hello ThousandEyes tile in hello Access Panel, you should get automatically signed-on tooyour ThousandEyes application.</span></span>

<span data-ttu-id="92000-249">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="92000-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92000-250">További források</span><span class="sxs-lookup"><span data-stu-id="92000-250">Additional resources</span></span>

* [<span data-ttu-id="92000-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="92000-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="92000-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="92000-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

