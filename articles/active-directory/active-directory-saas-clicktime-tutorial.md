---
title: "Oktatóanyag: Azure Active Directoryval integrált ClickTime |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ClickTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: a0259e31164cad6c6c77ed8aac1c50cd9a3e46ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="bc0f3-103">Oktatóanyag: Azure Active Directoryval integrált ClickTime</span><span class="sxs-lookup"><span data-stu-id="bc0f3-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="bc0f3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ClickTime az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bc0f3-104">In this tutorial, you learn how toointegrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bc0f3-105">ClickTime integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-105">Integrating ClickTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bc0f3-106">Megadhatja a hozzáférés tooClickTime rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bc0f3-106">You can control in Azure AD who has access tooClickTime</span></span>
- <span data-ttu-id="bc0f3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooClickTime (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bc0f3-107">You can enable your users tooautomatically get signed-on tooClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bc0f3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bc0f3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bc0f3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bc0f3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc0f3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bc0f3-110">Prerequisites</span></span>

<span data-ttu-id="bc0f3-111">az Azure AD integrálása ClickTime tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-111">tooconfigure Azure AD integration with ClickTime, you need hello following items:</span></span>

- <span data-ttu-id="bc0f3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bc0f3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bc0f3-113">Egy ClickTime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bc0f3-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bc0f3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bc0f3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bc0f3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bc0f3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc0f3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bc0f3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-118">Scenario description</span></span>
<span data-ttu-id="bc0f3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bc0f3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bc0f3-121">Hello gyűjteményből ClickTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-121">Adding ClickTime from hello gallery</span></span>
2. <span data-ttu-id="bc0f3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bc0f3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-hello-gallery"></a><span data-ttu-id="bc0f3-123">Hello gyűjteményből ClickTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-123">Adding ClickTime from hello gallery</span></span>
<span data-ttu-id="bc0f3-124">tooconfigure hello integrációja ClickTime az Azure AD-be, meg kell tooadd ClickTime hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-124">tooconfigure hello integration of ClickTime into Azure AD, you need tooadd ClickTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bc0f3-125">**tooadd ClickTime hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-125">**tooadd ClickTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc0f3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="bc0f3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bc0f3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="bc0f3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="bc0f3-133">Hello keresési mezőbe, írja be a **ClickTime**, jelölje be **ClickTime** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-133">In hello search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bc0f3-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="bc0f3-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ClickTime.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bc0f3-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ClickTime tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ClickTime is tooa user in Azure AD.</span></span> <span data-ttu-id="bc0f3-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ClickTime közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-138">In other words, a link relationship between an Azure AD user and hello related user in ClickTime needs toobe established.</span></span>

<span data-ttu-id="bc0f3-139">ClickTime, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-139">In ClickTime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bc0f3-140">tooconfigure és az Azure AD az egyszeri bejelentkezés ClickTime-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-140">tooconfigure and test Azure AD single sign-on with ClickTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bc0f3-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bc0f3-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bc0f3-143">**[ClickTime tesztfelhasználó létrehozása](#create-a-clicktime-test-user)**  -toohave egy megfelelője a Britta Simon a ClickTime, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - toohave a counterpart of Britta Simon in ClickTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bc0f3-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bc0f3-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bc0f3-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bc0f3-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ClickTime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="bc0f3-148">**az Azure AD tooconfigure egyszeri bejelentkezést a ClickTime, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-148">**tooconfigure Azure AD single sign-on with ClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc0f3-149">Az Azure portál, a hello hello **ClickTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-149">In hello Azure portal, on hello **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="bc0f3-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="bc0f3-153">A hello **ClickTime tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-153">On hello **ClickTime Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk ClickTime tartomány és az URL-címek](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="bc0f3-155">a.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-155">a.</span></span> <span data-ttu-id="bc0f3-156">A hello **azonosító** szövegmező, adja meg az URL-címet:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="bc0f3-156">In hello **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="bc0f3-157">b.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-157">b.</span></span> <span data-ttu-id="bc0f3-158">A hello **válasz URL-CÍMEN** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-158">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="bc0f3-159">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="bc0f3-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bc0f3-163">A hello **ClickTime konfigurációs** kattintson **konfigurálása ClickTime** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-163">On hello **ClickTime Configuration** section, click **Configure ClickTime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bc0f3-164">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ClickTime konfiguráció](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="bc0f3-166">Egy másik webes böngészőablakban jelentkezzen be a ClickTime vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="bc0f3-167">Hello hello felső eszköztárán kattintson **beállítások**, és kattintson a **biztonsági beállítások**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-167">In hello toolbar on hello top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="bc0f3-168">A hello **egyszeri bejelentkezési beállítások** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-168">In hello **Single Sign-On Preferences** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="bc0f3-169">![Biztonsági beállítások](./media/active-directory-saas-clicktime-tutorial/tic777280.png "biztonsági beállítások")</span><span class="sxs-lookup"><span data-stu-id="bc0f3-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="bc0f3-170">a.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-170">a.</span></span>  <span data-ttu-id="bc0f3-171">Válassza ki **engedélyezése** jelentkezzen be egyszeri bejelentkezés (SSO) használatával **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="bc0f3-172">b.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-172">b.</span></span> <span data-ttu-id="bc0f3-173">A hello **identitás szolgáltatói végpont** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-173">In hello **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="bc0f3-174">c.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-174">c.</span></span>  <span data-ttu-id="bc0f3-175">Nyissa meg hello **base-64 kódolású tanúsítvány** az Azure portálról letöltött **Jegyzettömb**hello tartalmat másolja és illessze be hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-175">Open hello **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="bc0f3-176">d.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-176">d.</span></span>  <span data-ttu-id="bc0f3-177">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bc0f3-178">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bc0f3-178">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bc0f3-179">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-179">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bc0f3-180">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bc0f3-180">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bc0f3-181">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="bc0f3-181">Create an Azure AD test user</span></span>
<span data-ttu-id="bc0f3-182">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-182">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="bc0f3-184">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-184">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc0f3-185">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-185">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bc0f3-187">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-187">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bc0f3-189">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-189">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bc0f3-191">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-191">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bc0f3-193">a.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-193">a.</span></span> <span data-ttu-id="bc0f3-194">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-194">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bc0f3-195">b.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-195">b.</span></span> <span data-ttu-id="bc0f3-196">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-196">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bc0f3-197">c.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-197">c.</span></span> <span data-ttu-id="bc0f3-198">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-198">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bc0f3-199">d.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-199">d.</span></span> <span data-ttu-id="bc0f3-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="bc0f3-201">ClickTime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc0f3-201">Create a ClickTime test user</span></span>

<span data-ttu-id="bc0f3-202">A sorrend tooenable az Azure AD felhasználók toolog ClickTime be azok ki kell építenie ClickTime be.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-202">In order tooenable Azure AD users toolog into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="bc0f3-203">ClickTime hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-203">In hello case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="bc0f3-204">Bármely más ClickTime felhasználói fiók létrehozása eszközök vagy ClickTime tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime tooprovision Azure AD user accounts.</span></span>

<span data-ttu-id="bc0f3-205">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-205">**tooprovision a user account, perform hello following steps:**</span></span>
1. <span data-ttu-id="bc0f3-206">Jelentkezzen be tooyour **ClickTime** bérlő.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-206">Log in tooyour **ClickTime** tenant.</span></span>
2. <span data-ttu-id="bc0f3-207">Hello hello felső eszköztárán kattintson **vállalati**, és kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-207">In hello toolbar on hello top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="bc0f3-208">![Személyek](./media/active-directory-saas-clicktime-tutorial/tic777282.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="bc0f3-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="bc0f3-209">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="bc0f3-210">![Adja hozzá a személy](./media/active-directory-saas-clicktime-tutorial/tic777283.png "személy hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="bc0f3-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="bc0f3-211">Az új személy szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="bc0f3-211">In hello New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="bc0f3-212">![Személyek](./media/active-directory-saas-clicktime-tutorial/tic777284.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="bc0f3-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="bc0f3-213">a.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-213">a.</span></span>  <span data-ttu-id="bc0f3-214">A hello **teljes név** szövegmező, például a felhasználó teljes név típusa **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-214">In hello **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="bc0f3-215">b.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-215">b.</span></span>  <span data-ttu-id="bc0f3-216">A hello **e-mail cím** szövegmezőhöz: hello e-mail felhasználó például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="bc0f3-216">In hello **email address** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="bc0f3-217">Ha szeretné, beállíthatja a további tulajdonságok hello új személy objektum.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-217">If you want to, you can set additional properties of hello new person object.</span></span>
   
    <span data-ttu-id="bc0f3-218">c.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-218">c.</span></span>  <span data-ttu-id="bc0f3-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-219">Click **Save**.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="bc0f3-220">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="bc0f3-220">Assign hello Azure AD test user</span></span>

<span data-ttu-id="bc0f3-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooClickTime megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooClickTime.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="bc0f3-223">**tooassign Britta Simon tooClickTime, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bc0f3-223">**tooassign Britta Simon tooClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc0f3-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bc0f3-226">Hello alkalmazások listában válassza ki a **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-226">In hello applications list, select **ClickTime**.</span></span>

    ![Hello alkalmazások listáját a ClickTimne hivatkozás](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="bc0f3-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="bc0f3-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-230">Click **Add** button.</span></span> <span data-ttu-id="bc0f3-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="bc0f3-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bc0f3-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bc0f3-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bc0f3-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bc0f3-236">Test single sign-on</span></span>

<span data-ttu-id="bc0f3-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bc0f3-238">Ha a hozzáférési Panel hello hello ClickTime csempe gombra kattint, automatikusan bejelentkezett tooyour ClickTime alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="bc0f3-238">When you click hello ClickTime tile in hello Access Panel, you should get automatically signed-on tooyour ClickTime application.</span></span>
<span data-ttu-id="bc0f3-239">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bc0f3-239">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc0f3-240">További források</span><span class="sxs-lookup"><span data-stu-id="bc0f3-240">Additional resources</span></span>

* [<span data-ttu-id="bc0f3-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bc0f3-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bc0f3-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bc0f3-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

