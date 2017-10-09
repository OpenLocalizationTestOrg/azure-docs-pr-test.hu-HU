---
title: "Oktatóanyag: Azure Active Directoryval integrált ThirdLight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ThirdLight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a510e514f6a8c4e89220b9a6f6db29668b451b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a><span data-ttu-id="ffd3d-103">Oktatóanyag: Azure Active Directoryval integrált ThirdLight</span><span class="sxs-lookup"><span data-stu-id="ffd3d-103">Tutorial: Azure Active Directory integration with ThirdLight</span></span>

<span data-ttu-id="ffd3d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ThirdLight az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ffd3d-104">In this tutorial, you learn how toointegrate ThirdLight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ffd3d-105">ThirdLight integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-105">Integrating ThirdLight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ffd3d-106">Megadhatja a hozzáférés tooThirdLight rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ffd3d-106">You can control in Azure AD who has access tooThirdLight</span></span>
- <span data-ttu-id="ffd3d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooThirdLight (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ffd3d-107">You can enable your users tooautomatically get signed-on tooThirdLight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ffd3d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ffd3d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ffd3d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ffd3d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ffd3d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ffd3d-110">Prerequisites</span></span>

<span data-ttu-id="ffd3d-111">az Azure AD integrálása ThirdLight tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-111">tooconfigure Azure AD integration with ThirdLight, you need hello following items:</span></span>

- <span data-ttu-id="ffd3d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ffd3d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ffd3d-113">Egy ThirdLight egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ffd3d-113">A ThirdLight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ffd3d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ffd3d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ffd3d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ffd3d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ffd3d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ffd3d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-118">Scenario description</span></span>
<span data-ttu-id="ffd3d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ffd3d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ffd3d-121">Hello gyűjteményből ThirdLight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-121">Adding ThirdLight from hello gallery</span></span>
2. <span data-ttu-id="ffd3d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ffd3d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thirdlight-from-hello-gallery"></a><span data-ttu-id="ffd3d-123">Hello gyűjteményből ThirdLight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-123">Adding ThirdLight from hello gallery</span></span>
<span data-ttu-id="ffd3d-124">tooconfigure hello integrációja ThirdLight az Azure AD-be, meg kell tooadd ThirdLight hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-124">tooconfigure hello integration of ThirdLight into Azure AD, you need tooadd ThirdLight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ffd3d-125">**tooadd ThirdLight hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ffd3d-125">**tooadd ThirdLight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ffd3d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ffd3d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ffd3d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ffd3d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ffd3d-133">Hello keresési mezőbe, írja be a **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-133">In hello search box, type **ThirdLight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. <span data-ttu-id="ffd3d-135">A hello eredmények panelen válassza ki a **ThirdLight**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-135">In hello results panel, select **ThirdLight**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ffd3d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ffd3d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ffd3d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján ThirdLight</span><span class="sxs-lookup"><span data-stu-id="ffd3d-138">In this section, you configure and test Azure AD single sign-on with ThirdLight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ffd3d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ThirdLight tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ThirdLight is tooa user in Azure AD.</span></span> <span data-ttu-id="ffd3d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ThirdLight közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-140">In other words, a link relationship between an Azure AD user and hello related user in ThirdLight needs toobe established.</span></span>

<span data-ttu-id="ffd3d-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ThirdLight.</span></span>

<span data-ttu-id="ffd3d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ThirdLight-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-142">tooconfigure and test Azure AD single sign-on with ThirdLight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ffd3d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ffd3d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ffd3d-145">**[ThirdLight tesztfelhasználó létrehozása](#creating-a-thirdlight-test-user)**  -toohave egy megfelelője a Britta Simon a ThirdLight, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-145">**[Creating a ThirdLight test user](#creating-a-thirdlight-test-user)** - toohave a counterpart of Britta Simon in ThirdLight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ffd3d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ffd3d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ffd3d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ffd3d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ThirdLight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ThirdLight application.</span></span>

<span data-ttu-id="ffd3d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ThirdLight, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ffd3d-150">**tooconfigure Azure AD single sign-on with ThirdLight, perform hello following steps:**</span></span>

1. <span data-ttu-id="ffd3d-151">Az Azure portál, a hello hello **ThirdLight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-151">In hello Azure portal, on hello **ThirdLight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ffd3d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. <span data-ttu-id="ffd3d-155">A hello **ThirdLight tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-155">On hello **ThirdLight Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    <span data-ttu-id="ffd3d-157">a.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-157">a.</span></span> <span data-ttu-id="ffd3d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.thirdlight.com/`</span><span class="sxs-lookup"><span data-stu-id="ffd3d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.thirdlight.com/`</span></span>

    <span data-ttu-id="ffd3d-159">b.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-159">b.</span></span> <span data-ttu-id="ffd3d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.thirdlight.com/saml/sp`</span><span class="sxs-lookup"><span data-stu-id="ffd3d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.thirdlight.com/saml/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ffd3d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-161">These values are not real.</span></span> <span data-ttu-id="ffd3d-162">Frissítse a bejelentkezési URL-cím és Identiifer tényleges hello az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-162">Update these values with hello actual Sign-On URL and Identiifer.</span></span> <span data-ttu-id="ffd3d-163">Ügyfél [ThirdLight ügyfél-támogatási csoport](https://www.thirdlight.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-163">Contact [ThirdLight Client support team](https://www.thirdlight.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="ffd3d-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. <span data-ttu-id="ffd3d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ffd3d-168">Egy másik webes böngészőablakban jelentkezzen tooyour ThirdLight vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-168">In a different web browser window, log in tooyour ThirdLight company site as an administrator.</span></span>

7. <span data-ttu-id="ffd3d-169">Nyissa meg túl**konfigurációs \> rendszerfelügyelet**, és kattintson a **egy SAML2**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-169">Go too**Configuration \> System Administration**, and then click **SAML2**.</span></span>
   
    <span data-ttu-id="ffd3d-170">![Rendszer-felügyeleti](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="ffd3d-170">![System Administration](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "System Administration")</span></span>

8. <span data-ttu-id="ffd3d-171">Hello egy SAML2 konfigurációs szakaszban hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-171">In hello SAML2 configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="ffd3d-172">![SAML-alapú egyszeri bejelentkezést](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML-alapú egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="ffd3d-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML Single Sign-On")</span></span>   

     <span data-ttu-id="ffd3d-173">a.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-173">a.</span></span> <span data-ttu-id="ffd3d-174">Válassza ki **egy SAML2 egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-174">Select **Enable SAML2 Single Sign-On**.</span></span>
 
     <span data-ttu-id="ffd3d-175">b.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-175">b.</span></span> <span data-ttu-id="ffd3d-176">Mint **IdP metaadatok forrást**, jelölje be **terhelés IdP metaadatok XML-fájljából**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-176">As **Source for IdP Metadata**, select **Load IdP Metadata from XML**.</span></span>
 
     <span data-ttu-id="ffd3d-177">c.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-177">c.</span></span> <span data-ttu-id="ffd3d-178">Nyissa meg a letöltött hello metaadatfájl hello tartalmat másolja és illessze be hello **IdP metaadatainak XML-kódja** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-178">Open hello downloaded metadata file, copy hello content, and then paste it into hello **IdP Metadata XML** textbox.</span></span> 
     
     <span data-ttu-id="ffd3d-179">d.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-179">d.</span></span> <span data-ttu-id="ffd3d-180">Kattintson a **menteni egy SAML2 beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-180">Click **Save SAML2 settings**.</span></span>

> [!TIP]
> <span data-ttu-id="ffd3d-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ffd3d-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ffd3d-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ffd3d-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ffd3d-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ffd3d-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="ffd3d-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ffd3d-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ffd3d-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ffd3d-188">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ffd3d-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ffd3d-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ffd3d-194">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ffd3d-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ffd3d-196">a.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-196">a.</span></span> <span data-ttu-id="ffd3d-197">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ffd3d-198">b.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-198">b.</span></span> <span data-ttu-id="ffd3d-199">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-199">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="ffd3d-200">c.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-200">c.</span></span> <span data-ttu-id="ffd3d-201">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ffd3d-202">d.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-202">d.</span></span> <span data-ttu-id="ffd3d-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-203">Click **Create**.</span></span>
 
### <a name="creating-a-thirdlight-test-user"></a><span data-ttu-id="ffd3d-204">ThirdLight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffd3d-204">Creating a ThirdLight test user</span></span>

<span data-ttu-id="ffd3d-205">az Azure AD tooenable felhasználók toolog a tooThirdLight, akkor ki kell építenie ThirdLight be.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-205">tooenable Azure AD users toolog in tooThirdLight, they must be provisioned into ThirdLight.</span></span>  
<span data-ttu-id="ffd3d-206">ThirdLight hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-206">In hello case of ThirdLight, provisioning is a manual task.</span></span>

<span data-ttu-id="ffd3d-207">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ffd3d-207">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="ffd3d-208">Jelentkezzen be tooyour **ThirdLight** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-208">Log in tooyour **ThirdLight** company site as an administrator.</span></span>

2. <span data-ttu-id="ffd3d-209">Nyissa meg túl**felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-209">Go too**Users** tab.</span></span>

3. <span data-ttu-id="ffd3d-210">Válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-210">Select **Users and Groups**.</span></span>

4. <span data-ttu-id="ffd3d-211">Kattintson a **új felhasználó hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-211">Click **Add new User** button.</span></span>

5. <span data-ttu-id="ffd3d-212">Adja meg **hello felhasználónév, a neve és leírása, e-mailek, válasszon egy előre definiált vagy új csoporttagok** tooprovision kívánt fiók érvényes aad-ben.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-212">Enter **hello Username, Name or Description, Email, Choose a Preset or Group of New Members** of a valid AAD account you want tooprovision.</span></span>

6. <span data-ttu-id="ffd3d-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-213">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="ffd3d-214">Bármely más Thirdlight felhasználói fiók létrehozása eszközök vagy Thirdlight tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-214">You can use any other Thirdlight user account creation tools or APIs provided by Thirdlight tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ffd3d-215">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ffd3d-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ffd3d-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooThirdLight megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThirdLight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ffd3d-218">**tooassign Britta Simon tooThirdLight, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ffd3d-218">**tooassign Britta Simon tooThirdLight, perform hello following steps:**</span></span>

1. <span data-ttu-id="ffd3d-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ffd3d-221">Hello alkalmazások listában válassza ki a **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-221">In hello applications list, select **ThirdLight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. <span data-ttu-id="ffd3d-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ffd3d-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-225">Click **Add** button.</span></span> <span data-ttu-id="ffd3d-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ffd3d-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ffd3d-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ffd3d-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ffd3d-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ffd3d-231">Testing single sign-on</span></span>

<span data-ttu-id="ffd3d-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ffd3d-233">Ha a hozzáférési Panel hello hello ThirdLight csempe gombra kattint, automatikusan bejelentkezett tooyour ThirdLight alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="ffd3d-233">When you click hello ThirdLight tile in hello Access Panel, you should get automatically signed-on tooyour ThirdLight application.</span></span>
<span data-ttu-id="ffd3d-234">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ffd3d-234">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ffd3d-235">További források</span><span class="sxs-lookup"><span data-stu-id="ffd3d-235">Additional resources</span></span>

* [<span data-ttu-id="ffd3d-236">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ffd3d-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ffd3d-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ffd3d-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png

