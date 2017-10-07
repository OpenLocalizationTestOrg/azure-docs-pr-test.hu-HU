---
title: "Oktatóanyag: Azure Active Directoryval integrált Asana |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Asana között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="e2c65-103">Oktatóanyag: Azure Active Directoryval integrált Asana</span><span class="sxs-lookup"><span data-stu-id="e2c65-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="e2c65-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Asana az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e2c65-104">In this tutorial, you learn how toointegrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e2c65-105">Asana integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e2c65-105">Integrating Asana with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e2c65-106">Megadhatja a hozzáférés tooAsana rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e2c65-106">You can control in Azure AD who has access tooAsana</span></span>
- <span data-ttu-id="e2c65-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAsana (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e2c65-107">You can enable your users tooautomatically get signed-on tooAsana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e2c65-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e2c65-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e2c65-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2c65-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2c65-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e2c65-110">Prerequisites</span></span>

<span data-ttu-id="e2c65-111">az Azure AD integrálása Asana tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e2c65-111">tooconfigure Azure AD integration with Asana, you need hello following items:</span></span>

- <span data-ttu-id="e2c65-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e2c65-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e2c65-113">Egy Asana egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e2c65-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2c65-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e2c65-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e2c65-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e2c65-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e2c65-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2c65-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e2c65-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2c65-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e2c65-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e2c65-118">Scenario description</span></span>
<span data-ttu-id="e2c65-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e2c65-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e2c65-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e2c65-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e2c65-121">Hello gyűjteményből Asana hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2c65-121">Adding Asana from hello gallery</span></span>
2. <span data-ttu-id="e2c65-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e2c65-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-hello-gallery"></a><span data-ttu-id="e2c65-123">Hello gyűjteményből Asana hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2c65-123">Adding Asana from hello gallery</span></span>
<span data-ttu-id="e2c65-124">tooconfigure hello integrációja Asana az Azure AD-be, meg kell tooadd Asana hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e2c65-124">tooconfigure hello integration of Asana into Azure AD, you need tooadd Asana from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e2c65-125">**tooadd Asana hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e2c65-125">**tooadd Asana from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e2c65-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="e2c65-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e2c65-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e2c65-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e2c65-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="e2c65-133">Hello keresési mezőbe, írja be a **Asana**, jelölje be **Asana** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-133">In hello search box, type **Asana**, select **Asana** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e2c65-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2c65-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e2c65-136">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Asana</span><span class="sxs-lookup"><span data-stu-id="e2c65-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e2c65-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Asana tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e2c65-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Asana is tooa user in Azure AD.</span></span> <span data-ttu-id="e2c65-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Asana közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e2c65-138">In other words, a link relationship between an Azure AD user and hello related user in Asana needs toobe established.</span></span>

<span data-ttu-id="e2c65-139">Asana, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e2c65-139">In Asana, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e2c65-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Asana-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e2c65-140">tooconfigure and test Azure AD single sign-on with Asana, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e2c65-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e2c65-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e2c65-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2c65-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2c65-143">**[Hozzon létre egy Asana tesztfelhasználó](#create-an-asana-test-user)**  -toohave egy megfelelője a Britta Simon a Asana, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e2c65-143">**[Create an Asana test user](#create-an-asana-test-user)** - toohave a counterpart of Britta Simon in Asana that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e2c65-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e2c65-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2c65-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e2c65-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e2c65-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2c65-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e2c65-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Asana alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2c65-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="e2c65-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Asana, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e2c65-148">**tooconfigure Azure AD single sign-on with Asana, perform hello following steps:**</span></span>

1. <span data-ttu-id="e2c65-149">Az Azure portál, a hello hello **Asana** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-149">In hello Azure portal, on hello **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e2c65-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e2c65-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="e2c65-153">A hello **Asana tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e2c65-153">On hello **Asana Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Asana tartomány és az URL-címek](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="e2c65-155">a.</span><span class="sxs-lookup"><span data-stu-id="e2c65-155">a.</span></span> <span data-ttu-id="e2c65-156">A hello **bejelentkezési URL-cím** szövegmezőhöz típus URL-címe:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="e2c65-156">In hello **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="e2c65-157">b.</span><span class="sxs-lookup"><span data-stu-id="e2c65-157">b.</span></span> <span data-ttu-id="e2c65-158">A hello **azonosító** szövegmezőhöz objektumtípus-érték:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="e2c65-158">In hello **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="e2c65-159">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e2c65-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="e2c65-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e2c65-163">A hello **Asana konfigurációs** kattintson **konfigurálása Asana** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e2c65-163">On hello **Asana Configuration** section, click **Configure Asana** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e2c65-164">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e2c65-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Asana konfiguráció](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="e2c65-166">Egy másik böngészőablakban, bejelentkezés tooyour Asana alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2c65-166">In a different browser window, sign-on tooyour Asana application.</span></span> <span data-ttu-id="e2c65-167">tooconfigure SSO Asana, hozzáférési hello munkaterület beállításainak a hello jobb felső sarkában üdvözlő képernyőt hello munkaterület nevére kattint.</span><span class="sxs-lookup"><span data-stu-id="e2c65-167">tooconfigure SSO in Asana, access hello workspace settings by clicking hello workspace name on hello top right corner of hello screen.</span></span> <span data-ttu-id="e2c65-168">Kattintson a  **\<a munkaterület neve\> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Asana egyszeri bejelentkezési beállítások](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="e2c65-170">A hello **szervezeti beállítások** ablak, kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-170">On hello **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="e2c65-171">Kattintson a **tagok SAML keresztül kell bejelentkezniük** tooenable hello SSO konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="e2c65-171">Then, click **Members must log in via SAML** tooenable hello SSO configuration.</span></span> <span data-ttu-id="e2c65-172">hello hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="e2c65-172">hello perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés szervezeti beállítások konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="e2c65-174">a.</span><span class="sxs-lookup"><span data-stu-id="e2c65-174">a.</span></span> <span data-ttu-id="e2c65-175">A hello **bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-175">In hello **Sign-in page URL** textbox, paste hello **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="e2c65-176">b.</span><span class="sxs-lookup"><span data-stu-id="e2c65-176">b.</span></span> <span data-ttu-id="e2c65-177">Kattintson a jobb gombbal az Azure portálról letöltött hello tanúsítvány, majd nyissa meg a Jegyzettömb vagy az előnyben részesített szövegszerkesztőben hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="e2c65-177">Right click hello certificate downloaded from Azure portal, then open hello certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="e2c65-178">Másolás hello tartalom közötti hello megkezdéséhez és end tanúsítvány cím hello és beillesztheti hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e2c65-178">Copy hello content between hello begin and hello end certificate title and paste it in hello **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="e2c65-179">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-179">Click **Save**.</span></span> <span data-ttu-id="e2c65-180">Nyissa meg túl[Asana útmutató egyszeri bejelentkezés beállításával kapcsolatos](https://asana.com/guide/help/premium/authentication#gl-saml) Ha segítségre van szüksége további.</span><span class="sxs-lookup"><span data-stu-id="e2c65-180">Go too[Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="e2c65-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e2c65-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e2c65-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e2c65-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e2c65-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e2c65-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e2c65-184">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e2c65-184">Create an Azure AD test user</span></span>

<span data-ttu-id="e2c65-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e2c65-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e2c65-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e2c65-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e2c65-188">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e2c65-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e2c65-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2c65-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e2c65-194">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e2c65-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e2c65-196">a.</span><span class="sxs-lookup"><span data-stu-id="e2c65-196">a.</span></span> <span data-ttu-id="e2c65-197">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e2c65-198">b.</span><span class="sxs-lookup"><span data-stu-id="e2c65-198">b.</span></span> <span data-ttu-id="e2c65-199">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e2c65-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e2c65-200">c.</span><span class="sxs-lookup"><span data-stu-id="e2c65-200">c.</span></span> <span data-ttu-id="e2c65-201">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e2c65-202">d.</span><span class="sxs-lookup"><span data-stu-id="e2c65-202">d.</span></span> <span data-ttu-id="e2c65-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="e2c65-204">Hozzon létre egy Asana tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e2c65-204">Create an Asana test user</span></span>

<span data-ttu-id="e2c65-205">Ebben a szakaszban egy Asana Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e2c65-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="e2c65-206">A **Asana**, nyissa meg toohello **csapatok** szakasz hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="e2c65-206">On **Asana**, go toohello **Teams** section on hello left panel.</span></span> <span data-ttu-id="e2c65-207">Hello kattintson, valamint a Bejelentkezés gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-207">Click hello plus sign button.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="e2c65-209">Írja be az üdvözlő e-mail britta.simon@contoso.com a hello szövegmezőbe, és válassza ki **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-209">Type hello email britta.simon@contoso.com in hello text box and then select **Invite**.</span></span>

3. <span data-ttu-id="e2c65-210">Kattintson a **küldése a meghívás**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-210">Click **Send Invite**.</span></span> <span data-ttu-id="e2c65-211">hello új felhasználó kap egy e-mailt az e-mailek figyelembe.</span><span class="sxs-lookup"><span data-stu-id="e2c65-211">hello new user will receive an email into her email account.</span></span> <span data-ttu-id="e2c65-212">Egyenként kell toocreate, és hello fiók érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="e2c65-212">She will need toocreate and validate hello account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e2c65-213">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e2c65-213">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e2c65-214">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAsana megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e2c65-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAsana.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="e2c65-216">**tooassign Britta Simon tooAsana, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e2c65-216">**tooassign Britta Simon tooAsana, perform hello following steps:**</span></span>

1. <span data-ttu-id="e2c65-217">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e2c65-219">Hello alkalmazások listában válassza ki a **Asana**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-219">In hello applications list, select **Asana**.</span></span>

    ![hello Asana hivatkozásra hello alkalmazások listája](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="e2c65-221">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="e2c65-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e2c65-223">Click **Add** button.</span></span> <span data-ttu-id="e2c65-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2c65-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e2c65-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e2c65-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e2c65-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2c65-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e2c65-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2c65-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e2c65-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e2c65-229">Test single sign-on</span></span>

<span data-ttu-id="e2c65-230">Ez a szakasz hello célját tootest van az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e2c65-230">hello objective of this section is tootest your Azure AD single sign-on.</span></span>

<span data-ttu-id="e2c65-231">Nyissa meg tooAsana bejelentkezési lapot.</span><span class="sxs-lookup"><span data-stu-id="e2c65-231">Go tooAsana login page.</span></span> <span data-ttu-id="e2c65-232">Hello E-mail cím szövegmezőben, hello e-mail cím beszúrása britta.simon@contoso.com. Hello jelszó szövegmezőjét hagyja üresen, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e2c65-232">In hello Email address textbox, insert hello email address britta.simon@contoso.com. Leave hello password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="e2c65-233">Átirányított tooAzure AD bejelentkezési oldal lesz.</span><span class="sxs-lookup"><span data-stu-id="e2c65-233">You will be redirected tooAzure AD login page.</span></span> <span data-ttu-id="e2c65-234">Végezze el az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="e2c65-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="e2c65-235">Most jelentkezett Asana.</span><span class="sxs-lookup"><span data-stu-id="e2c65-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2c65-236">További források</span><span class="sxs-lookup"><span data-stu-id="e2c65-236">Additional resources</span></span>

* [<span data-ttu-id="e2c65-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e2c65-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2c65-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e2c65-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
