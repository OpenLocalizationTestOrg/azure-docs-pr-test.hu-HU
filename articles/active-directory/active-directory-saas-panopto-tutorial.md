---
title: "Oktatóanyag: Azure Active Directoryval integrált Panopto |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Panopto között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 76b30e1cd2782bb5fba3d229378b8f82652b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a><span data-ttu-id="571b3-103">Oktatóanyag: Azure Active Directoryval integrált Panopto</span><span class="sxs-lookup"><span data-stu-id="571b3-103">Tutorial: Azure Active Directory integration with Panopto</span></span>

<span data-ttu-id="571b3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Panopto az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="571b3-104">In this tutorial, you learn how toointegrate Panopto with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="571b3-105">Panopto integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="571b3-105">Integrating Panopto with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="571b3-106">Megadhatja a hozzáférés tooPanopto rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="571b3-106">You can control in Azure AD who has access tooPanopto</span></span>
- <span data-ttu-id="571b3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPanopto (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="571b3-107">You can enable your users tooautomatically get signed-on tooPanopto (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="571b3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="571b3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="571b3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="571b3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="571b3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="571b3-110">Prerequisites</span></span>

<span data-ttu-id="571b3-111">az Azure AD integrálása Panopto tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="571b3-111">tooconfigure Azure AD integration with Panopto, you need hello following items:</span></span>

- <span data-ttu-id="571b3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="571b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="571b3-113">Egy Panopto egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="571b3-113">A Panopto single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="571b3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="571b3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="571b3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="571b3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="571b3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="571b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="571b3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="571b3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="571b3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="571b3-118">Scenario description</span></span>
<span data-ttu-id="571b3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="571b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="571b3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="571b3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="571b3-121">Hello gyűjteményből Panopto hozzáadása</span><span class="sxs-lookup"><span data-stu-id="571b3-121">Adding Panopto from hello gallery</span></span>
2. <span data-ttu-id="571b3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="571b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panopto-from-hello-gallery"></a><span data-ttu-id="571b3-123">Hello gyűjteményből Panopto hozzáadása</span><span class="sxs-lookup"><span data-stu-id="571b3-123">Adding Panopto from hello gallery</span></span>
<span data-ttu-id="571b3-124">tooconfigure hello integrációja Panopto az Azure AD-be, meg kell tooadd Panopto hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="571b3-124">tooconfigure hello integration of Panopto into Azure AD, you need tooadd Panopto from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="571b3-125">**tooadd Panopto hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="571b3-125">**tooadd Panopto from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="571b3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="571b3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="571b3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="571b3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="571b3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="571b3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="571b3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="571b3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="571b3-133">Hello keresési mezőbe, írja be a **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="571b3-133">In hello search box, type **Panopto**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_search.png)

5. <span data-ttu-id="571b3-135">A hello eredmények panelen válassza ki a **Panopto**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="571b3-135">In hello results panel, select **Panopto**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="571b3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="571b3-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="571b3-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Panopto</span><span class="sxs-lookup"><span data-stu-id="571b3-138">In this section, you configure and test Azure AD single sign-on with Panopto based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="571b3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Panopto tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="571b3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Panopto is tooa user in Azure AD.</span></span> <span data-ttu-id="571b3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Panopto közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="571b3-140">In other words, a link relationship between an Azure AD user and hello related user in Panopto needs toobe established.</span></span>

<span data-ttu-id="571b3-141">Panopto, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="571b3-141">In Panopto, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="571b3-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Panopto-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="571b3-142">tooconfigure and test Azure AD single sign-on with Panopto, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="571b3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="571b3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="571b3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="571b3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="571b3-145">**[Panopto tesztfelhasználó létrehozása](#creating-a-panopto-test-user)**  -toohave egy megfelelője a Britta Simon a Panopto, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="571b3-145">**[Creating a Panopto test user](#creating-a-panopto-test-user)** - toohave a counterpart of Britta Simon in Panopto that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="571b3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="571b3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="571b3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="571b3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="571b3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="571b3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="571b3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Panopto alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="571b3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Panopto application.</span></span>

<span data-ttu-id="571b3-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Panopto, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="571b3-150">**tooconfigure Azure AD single sign-on with Panopto, perform hello following steps:**</span></span>

1. <span data-ttu-id="571b3-151">Az Azure portál, a hello hello **Panopto** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="571b3-151">In hello Azure portal, on hello **Panopto** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="571b3-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="571b3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_samlbase.png)

3. <span data-ttu-id="571b3-155">A hello **Panopto tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="571b3-155">On hello **Panopto Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_url.png)

    <span data-ttu-id="571b3-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.panopto.com`</span><span class="sxs-lookup"><span data-stu-id="571b3-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.panopto.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="571b3-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="571b3-158">This value is not real.</span></span> <span data-ttu-id="571b3-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="571b3-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="571b3-160">Ügyfél [Panopto ügyfél-támogatási csoport](mailto:support@panopto.com‎) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="571b3-160">Contact [Panopto Client support team](mailto:support@panopto.com‎) tooget this value.</span></span> 
 
4. <span data-ttu-id="571b3-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="571b3-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_certificate.png) 

5. <span data-ttu-id="571b3-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="571b3-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="571b3-165">A hello **Panopto konfigurációs** kattintson **konfigurálása Panopto** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="571b3-165">On hello **Panopto Configuration** section, click **Configure Panopto** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="571b3-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="571b3-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_configure.png) 

7. <span data-ttu-id="571b3-168">Egy másik webes böngészőablakban jelentkezzen tooyour Panopto vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="571b3-168">In a different web browser window, log in tooyour Panopto company site as an administrator.</span></span>

8. <span data-ttu-id="571b3-169">Hello bal oldali hello eszköztárában kattintson **rendszer**, és kattintson a **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="571b3-169">In hello toolbar on hello left, click **System**, and then click **Identity Providers**.</span></span>
   
   <span data-ttu-id="571b3-170">![Rendszer](./media/active-directory-saas-panopto-tutorial/ic777670.png "rendszer")</span><span class="sxs-lookup"><span data-stu-id="571b3-170">![System](./media/active-directory-saas-panopto-tutorial/ic777670.png "System")</span></span>
9. <span data-ttu-id="571b3-171">Kattintson a **szolgáltató felvétele**.</span><span class="sxs-lookup"><span data-stu-id="571b3-171">Click **Add Provider**.</span></span>
   
   <span data-ttu-id="571b3-172">![Identitás-szolgáltatóktól](./media/active-directory-saas-panopto-tutorial/ic777671.png "identitás-szolgáltatóktól")</span><span class="sxs-lookup"><span data-stu-id="571b3-172">![Identity Providers](./media/active-directory-saas-panopto-tutorial/ic777671.png "Identity Providers")</span></span>
   
10. <span data-ttu-id="571b3-173">A SAML-szolgáltató szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="571b3-173">In hello SAML provider section, perform hello following steps:</span></span>
   
    <span data-ttu-id="571b3-174">![SaaS-konfiguráció](./media/active-directory-saas-panopto-tutorial/ic777672.png "Szolgáltatottszoftver-konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="571b3-174">![SaaS configuration](./media/active-directory-saas-panopto-tutorial/ic777672.png "SaaS configuration")</span></span>
    
    <span data-ttu-id="571b3-175">a.</span><span class="sxs-lookup"><span data-stu-id="571b3-175">a.</span></span> <span data-ttu-id="571b3-176">A hello **szolgáltatótípust** listáról válassza ki **SAML20**.</span><span class="sxs-lookup"><span data-stu-id="571b3-176">From hello **Provider Type** list, select **SAML20**.</span></span>    
    
    <span data-ttu-id="571b3-177">b.</span><span class="sxs-lookup"><span data-stu-id="571b3-177">b.</span></span> <span data-ttu-id="571b3-178">A hello **példánynév** szövegmezőhöz hello példányának nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="571b3-178">In hello **Instance Name** textbox, type a name for hello instance.</span></span>

    <span data-ttu-id="571b3-179">c.</span><span class="sxs-lookup"><span data-stu-id="571b3-179">c.</span></span> <span data-ttu-id="571b3-180">A hello **rövid leírás** szövegmező, írjon be egy rövid leírást.</span><span class="sxs-lookup"><span data-stu-id="571b3-180">In hello **Friendly Description** textbox, type a friendly description.</span></span>
    
    <span data-ttu-id="571b3-181">d.</span><span class="sxs-lookup"><span data-stu-id="571b3-181">d.</span></span> <span data-ttu-id="571b3-182">A **Golflabda lap URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="571b3-182">In **Bounce Page Url** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="571b3-183">e.</span><span class="sxs-lookup"><span data-stu-id="571b3-183">e.</span></span> <span data-ttu-id="571b3-184">A hello **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="571b3-184">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="571b3-185">f.</span><span class="sxs-lookup"><span data-stu-id="571b3-185">f.</span></span> <span data-ttu-id="571b3-186">Nyissa meg a base-64 kódolású tanúsítványt, amely már letöltötte az Azure portál, másolása hello tooyour vágólapon tartalma, és majd beillesztheti toohello **nyilvános kulcs** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="571b3-186">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy hello content of it in tooyour clipboard, and then paste it toohello **Public Key**  textbox.</span></span>

11. <span data-ttu-id="571b3-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="571b3-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="571b3-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="571b3-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="571b3-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="571b3-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="571b3-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="571b3-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="571b3-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="571b3-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="571b3-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="571b3-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="571b3-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="571b3-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="571b3-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="571b3-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="571b3-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="571b3-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="571b3-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="571b3-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="571b3-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="571b3-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panopto-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="571b3-203">a.</span><span class="sxs-lookup"><span data-stu-id="571b3-203">a.</span></span> <span data-ttu-id="571b3-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="571b3-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="571b3-205">b.</span><span class="sxs-lookup"><span data-stu-id="571b3-205">b.</span></span> <span data-ttu-id="571b3-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="571b3-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="571b3-207">c.</span><span class="sxs-lookup"><span data-stu-id="571b3-207">c.</span></span> <span data-ttu-id="571b3-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="571b3-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="571b3-209">d.</span><span class="sxs-lookup"><span data-stu-id="571b3-209">d.</span></span> <span data-ttu-id="571b3-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="571b3-210">Click **Create**.</span></span>
 
### <a name="creating-a-panopto-test-user"></a><span data-ttu-id="571b3-211">Panopto tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="571b3-211">Creating a Panopto test user</span></span>

<span data-ttu-id="571b3-212">Nincs a akkor tooconfigure felhasználók átadásához tooPanopto művelet elem.</span><span class="sxs-lookup"><span data-stu-id="571b3-212">There is no action item for you tooconfigure user provisioning tooPanopto.</span></span>  
<span data-ttu-id="571b3-213">Ha egy hozzárendelt felhasználó megpróbál a hozzáférési panelen hello tooPanopto toolog, Panopto ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="571b3-213">When an assigned user tries toolog in tooPanopto using hello access panel, Panopto checks whether hello user exists.</span></span>  

<span data-ttu-id="571b3-214">Nincs még nincs felhasználói fiók érhető el, ha a Panopto automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="571b3-214">If there is no user account available yet, it is automatically created by Panopto.</span></span>

>[!NOTE]
><span data-ttu-id="571b3-215">Bármely más Panopto felhasználói fiók létrehozása eszközök vagy Panopto tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="571b3-215">You can use any other Panopto user account creation tools or APIs provided by Panopto tooprovision Azure AD user accounts.</span></span>
>
>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="571b3-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="571b3-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="571b3-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPanopto megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="571b3-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPanopto.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="571b3-219">**tooassign Britta Simon tooPanopto, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="571b3-219">**tooassign Britta Simon tooPanopto, perform hello following steps:**</span></span>

1. <span data-ttu-id="571b3-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="571b3-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="571b3-222">Hello alkalmazások listában válassza ki a **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="571b3-222">In hello applications list, select **Panopto**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_app.png) 

3. <span data-ttu-id="571b3-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="571b3-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="571b3-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="571b3-226">Click **Add** button.</span></span> <span data-ttu-id="571b3-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="571b3-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="571b3-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="571b3-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="571b3-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="571b3-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="571b3-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="571b3-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="571b3-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="571b3-232">Testing single sign-on</span></span>

<span data-ttu-id="571b3-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="571b3-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="571b3-234">Ha a hozzáférési Panel hello hello Panopto csempe gombra kattint, szerezheti be automatikusan Panopto alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="571b3-234">When you click hello Panopto tile in hello Access Panel, you should get automatically login page of Panopto application.</span></span>
<span data-ttu-id="571b3-235">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="571b3-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="571b3-236">További források</span><span class="sxs-lookup"><span data-stu-id="571b3-236">Additional resources</span></span>

* [<span data-ttu-id="571b3-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="571b3-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="571b3-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="571b3-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_203.png

