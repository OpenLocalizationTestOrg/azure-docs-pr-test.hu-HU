---
title: "Oktatóanyag: Azure Active Directoryval integrált OpsGenie |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és OpsGenie között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 50d31c234fb9716d05d681b9bc4164740a3a662b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="5ceaf-103">Oktatóanyag: Azure Active Directoryval integrált OpsGenie</span><span class="sxs-lookup"><span data-stu-id="5ceaf-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="5ceaf-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate OpsGenie az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5ceaf-104">In this tutorial, you learn how toointegrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5ceaf-105">OpsGenie integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-105">Integrating OpsGenie with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5ceaf-106">Megadhatja a hozzáférés tooOpsGenie rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5ceaf-106">You can control in Azure AD who has access tooOpsGenie</span></span>
- <span data-ttu-id="5ceaf-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOpsGenie (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5ceaf-107">You can enable your users tooautomatically get signed-on tooOpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5ceaf-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5ceaf-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5ceaf-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5ceaf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ceaf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5ceaf-110">Prerequisites</span></span>

<span data-ttu-id="5ceaf-111">az Azure AD integrálása OpsGenie tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-111">tooconfigure Azure AD integration with OpsGenie, you need hello following items:</span></span>

- <span data-ttu-id="5ceaf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5ceaf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5ceaf-113">Egy OpsGenie egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5ceaf-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5ceaf-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5ceaf-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5ceaf-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5ceaf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ceaf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5ceaf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-118">Scenario description</span></span>
<span data-ttu-id="5ceaf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5ceaf-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5ceaf-121">Hello gyűjteményből OpsGenie hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-121">Adding OpsGenie from hello gallery</span></span>
2. <span data-ttu-id="5ceaf-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5ceaf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-hello-gallery"></a><span data-ttu-id="5ceaf-123">Hello gyűjteményből OpsGenie hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-123">Adding OpsGenie from hello gallery</span></span>
<span data-ttu-id="5ceaf-124">tooconfigure hello integrációja OpsGenie az Azure AD-be, meg kell tooadd OpsGenie hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-124">tooconfigure hello integration of OpsGenie into Azure AD, you need tooadd OpsGenie from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5ceaf-125">**tooadd OpsGenie hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5ceaf-125">**tooadd OpsGenie from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ceaf-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5ceaf-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5ceaf-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5ceaf-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5ceaf-133">Hello keresési mezőbe, írja be a **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-133">In hello search box, type **OpsGenie**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="5ceaf-135">A hello eredmények panelen válassza ki a **OpsGenie**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-135">In hello results panel, select **OpsGenie**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5ceaf-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5ceaf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5ceaf-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5ceaf-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó OpsGenie tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OpsGenie is tooa user in Azure AD.</span></span> <span data-ttu-id="5ceaf-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello OpsGenie közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-140">In other words, a link relationship between an Azure AD user and hello related user in OpsGenie needs toobe established.</span></span>

<span data-ttu-id="5ceaf-141">OpsGenie, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-141">In OpsGenie, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5ceaf-142">tooconfigure és az Azure AD az egyszeri bejelentkezés OpsGenie-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-142">tooconfigure and test Azure AD single sign-on with OpsGenie, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5ceaf-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5ceaf-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5ceaf-145">**[OpsGenie tesztfelhasználó létrehozása](#creating-a-opsgenie-test-user)**  -toohave egy megfelelője a Britta Simon a OpsGenie, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - toohave a counterpart of Britta Simon in OpsGenie that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5ceaf-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5ceaf-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5ceaf-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5ceaf-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az OpsGenie alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="5ceaf-150">**az Azure AD tooconfigure egyszeri bejelentkezést a OpsGenie, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5ceaf-150">**tooconfigure Azure AD single sign-on with OpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ceaf-151">Az Azure portál, a hello hello **OpsGenie** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-151">In hello Azure portal, on hello **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5ceaf-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="5ceaf-155">A hello **OpsGenie tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-155">On hello **OpsGenie Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="5ceaf-157">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="5ceaf-157">In hello **Sign-on URL** textbox, type hello URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="5ceaf-158">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="5ceaf-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5ceaf-162">A hello **OpsGenie konfigurációs** kattintson **konfigurálása OpsGenie** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-162">On hello **OpsGenie Configuration** section, click **Configure OpsGenie** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5ceaf-163">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5ceaf-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="5ceaf-165">Nyisson meg egy másik böngészőben példánya, és a majd bejelentkezést tooOpsGenie rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-165">Open another browser instance, and then log-in tooOpsGenie as an administrator.</span></span>

8. <span data-ttu-id="5ceaf-166">Kattintson a **beállítások**, majd kattintson a hello **egyszeri bejelentkezés** lapon.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-166">Click **Settings**, and then click hello **Single Sign On** tab.</span></span>
   
    ![OpsGenie egyszeri bejelentkezés](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="5ceaf-168">tooenable SSO, válassza ki **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-168">tooenable SSO, select **Enabled**.</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="5ceaf-170">A hello **szolgáltató** területen kattintson a hello **Azure Active Directory** fülre.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-170">In hello **Provider** section, click hello **Azure Active Directory** tab.</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="5ceaf-172">Hello Azure Active Directory párbeszédpanel lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-172">On hello Azure Active Directory dialog page, perform hello following steps:</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="5ceaf-174">a.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-174">a.</span></span> <span data-ttu-id="5ceaf-175">Beillesztés **egyszeri bejelentkezési szolgáltatás URL-cím**, amely akkor másolta, az Azure-portálon hello hello **SAML 2.0 végpont** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-175">Paste **Single Sign On Service URL**, which you have copied from hello Azure portal into hello **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="5ceaf-176">b.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-176">b.</span></span> <span data-ttu-id="5ceaf-177">Nyissa meg a letöltött base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be hello **X.500 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-177">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="5ceaf-178">c.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-178">c.</span></span> <span data-ttu-id="5ceaf-179">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="5ceaf-180">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5ceaf-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5ceaf-181">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5ceaf-182">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5ceaf-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5ceaf-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="5ceaf-184">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5ceaf-186">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5ceaf-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ceaf-187">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5ceaf-189">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5ceaf-191">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5ceaf-193">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5ceaf-195">a.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-195">a.</span></span> <span data-ttu-id="5ceaf-196">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5ceaf-197">b.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-197">b.</span></span> <span data-ttu-id="5ceaf-198">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5ceaf-199">c.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-199">c.</span></span> <span data-ttu-id="5ceaf-200">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5ceaf-201">d.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-201">d.</span></span> <span data-ttu-id="5ceaf-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="5ceaf-203">OpsGenie tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ceaf-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="5ceaf-204">hello ebben a szakaszban célja toocreate OpsGenie Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-204">hello objective of this section is toocreate a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="5ceaf-205">A webböngésző ablakának bejelentkezni a OpsGenie Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="5ceaf-206">Keresse meg a tooUsers lista kattintva **felhasználói** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-206">Navigate tooUsers list by clicking **User** in left panel.</span></span>
   
   ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="5ceaf-208">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-208">Click **Add User**.</span></span>

4. <span data-ttu-id="5ceaf-209">A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5ceaf-209">On hello **Add User** dialog, perform hello following steps:</span></span>
   
   ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="5ceaf-211">a.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-211">a.</span></span> <span data-ttu-id="5ceaf-212">A hello **E-mail** szövegmezőben, az Azure Active Directoryban címzett BrittaSimon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-212">In hello **Email** textbox, type hello email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="5ceaf-213">b.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-213">b.</span></span> <span data-ttu-id="5ceaf-214">A hello **teljes nevét** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-214">In hello **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="5ceaf-215">c.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-215">c.</span></span> <span data-ttu-id="5ceaf-216">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="5ceaf-217">Britta saját profil beállításával kapcsolatos utasításokat tartalmazó e-mailt kap.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5ceaf-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5ceaf-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5ceaf-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOpsGenie megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOpsGenie.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5ceaf-221">**tooassign Britta Simon tooOpsGenie, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5ceaf-221">**tooassign Britta Simon tooOpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ceaf-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5ceaf-224">Hello alkalmazások listában válassza ki a **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-224">In hello applications list, select **OpsGenie**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="5ceaf-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5ceaf-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-228">Click **Add** button.</span></span> <span data-ttu-id="5ceaf-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5ceaf-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5ceaf-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5ceaf-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5ceaf-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5ceaf-234">Testing single sign-on</span></span>

<span data-ttu-id="5ceaf-235">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="5ceaf-236">Ha a hozzáférési Panel hello hello OpsGenie csempe gombra kattint, automatikusan bejelentkezett tooyour OpsGenie alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="5ceaf-236">When you click hello OpsGenie tile in hello Access Panel, you should get automatically signed-on tooyour OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ceaf-237">További források</span><span class="sxs-lookup"><span data-stu-id="5ceaf-237">Additional resources</span></span>

* [<span data-ttu-id="5ceaf-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5ceaf-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5ceaf-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5ceaf-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

