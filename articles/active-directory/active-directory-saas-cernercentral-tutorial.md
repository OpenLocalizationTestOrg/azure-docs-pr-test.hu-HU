---
title: "Oktatóanyag: Azure Active Directoryval integrált Cerner központi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Cerner központi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 3493d180e8f229b7cd228769f780f10208114889
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="39d32-103">Oktatóanyag: Azure Active Directoryval integrált Cerner központi</span><span class="sxs-lookup"><span data-stu-id="39d32-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="39d32-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cerner központi az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="39d32-104">In this tutorial, you learn how toointegrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="39d32-105">Cerner központi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="39d32-105">Integrating Cerner Central with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="39d32-106">Megadhatja a központi hozzáférési tooCerner rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="39d32-106">You can control in Azure AD who has access tooCerner Central</span></span>
- <span data-ttu-id="39d32-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCerner központi (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="39d32-107">You can enable your users tooautomatically get signed-on tooCerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="39d32-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="39d32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="39d32-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="39d32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39d32-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="39d32-110">Prerequisites</span></span>

<span data-ttu-id="39d32-111">az Azure AD integrálása Cerner központi tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="39d32-111">tooconfigure Azure AD integration with Cerner Central, you need hello following items:</span></span>

- <span data-ttu-id="39d32-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="39d32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="39d32-113">Egy jóváhagyott Cerner központi rendszerfiók</span><span class="sxs-lookup"><span data-stu-id="39d32-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="39d32-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="39d32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="39d32-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="39d32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="39d32-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="39d32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="39d32-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="39d32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="39d32-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="39d32-118">Scenario description</span></span>
<span data-ttu-id="39d32-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="39d32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="39d32-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="39d32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="39d32-121">Hello gyűjteményből Cerner központi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39d32-121">Adding Cerner Central from hello gallery</span></span>
2. <span data-ttu-id="39d32-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="39d32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-hello-gallery"></a><span data-ttu-id="39d32-123">Hello gyűjteményből Cerner központi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39d32-123">Adding Cerner Central from hello gallery</span></span>
<span data-ttu-id="39d32-124">tooconfigure hello integrációja Cerner központi az Azure AD-be, meg kell tooadd Cerner központi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="39d32-124">tooconfigure hello integration of Cerner Central into Azure AD, you need tooadd Cerner Central from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="39d32-125">**tooadd Cerner központi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="39d32-125">**tooadd Cerner Central from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="39d32-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="39d32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="39d32-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="39d32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="39d32-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="39d32-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="39d32-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** gomb felett hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39d32-131">tooadd new application, click **New application** button on top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="39d32-133">Hello keresési mezőbe, írja be a **Cerner központi**.</span><span class="sxs-lookup"><span data-stu-id="39d32-133">In hello search box, type **Cerner Central**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="39d32-135">A hello eredmények panelen válassza ki a **Cerner központi**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="39d32-135">In hello results panel, select **Cerner Central**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="39d32-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="39d32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="39d32-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Cerner központi "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="39d32-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="39d32-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Cerner közép-India tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="39d32-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cerner Central is tooa user in Azure AD.</span></span> <span data-ttu-id="39d32-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Cerner közép-India közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="39d32-140">In other words, a link relationship between an Azure AD user and hello related user in Cerner Central needs toobe established.</span></span>

<span data-ttu-id="39d32-141">tooconfigure és az Azure AD az egyszeri bejelentkezés Cerner központi-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="39d32-141">tooconfigure and test Azure AD single sign-on with Cerner Central, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="39d32-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="39d32-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="39d32-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39d32-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="39d32-144">**[Cerner központi tesztfelhasználó létrehozása](#creating-a-cerner-central-test-user)**  -toohave egy megfelelője a Britta Simon Cerner közép, amely hello felhasználói csatolt toohello az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="39d32-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - toohave a counterpart of Britta Simon in Cerner Central that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="39d32-145">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="39d32-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="39d32-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="39d32-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="39d32-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39d32-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="39d32-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Cerner központi alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="39d32-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="39d32-149">**az Azure AD tooconfigure egyszeri bejelentkezést a Cerner központi hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="39d32-149">**tooconfigure Azure AD single sign-on with Cerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="39d32-150">Az Azure portál, a hello hello **Cerner központi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="39d32-150">In hello Azure portal, on hello **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="39d32-152">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="39d32-152">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="39d32-154">A hello **Cerner központi tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39d32-154">On hello **Cerner Central Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="39d32-156">a.</span><span class="sxs-lookup"><span data-stu-id="39d32-156">a.</span></span> <span data-ttu-id="39d32-157">A hello **azonosító** szövegmezőhöz típusú hello érték a következő minták hello használata:</span><span class="sxs-lookup"><span data-stu-id="39d32-157">In hello **Identifier** textbox, type hello value using hello following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="39d32-158">b.</span><span class="sxs-lookup"><span data-stu-id="39d32-158">b.</span></span> <span data-ttu-id="39d32-159">A hello **válasz URL-CÍMEN** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="39d32-159">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="39d32-160">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="39d32-160">These values are not hello real.</span></span> <span data-ttu-id="39d32-161">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="39d32-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="39d32-162">Ügyfél [Cerner központi támogatási csoport](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="39d32-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget these values.</span></span>
 
4. <span data-ttu-id="39d32-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="39d32-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="39d32-165">toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39d32-165">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="39d32-166">a.</span><span class="sxs-lookup"><span data-stu-id="39d32-166">a.</span></span> <span data-ttu-id="39d32-167">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="39d32-167">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="39d32-169">b.</span><span class="sxs-lookup"><span data-stu-id="39d32-169">b.</span></span> <span data-ttu-id="39d32-170">Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="39d32-170">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="39d32-172">c.</span><span class="sxs-lookup"><span data-stu-id="39d32-172">c.</span></span> <span data-ttu-id="39d32-173">Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="39d32-173">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="39d32-175">d.</span><span class="sxs-lookup"><span data-stu-id="39d32-175">d.</span></span> <span data-ttu-id="39d32-176">Most lépjen tulajdonságlapján toohello **Cerner központi** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="39d32-176">Now go toohello property page of **Cerner Central** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="39d32-178">e.</span><span class="sxs-lookup"><span data-stu-id="39d32-178">e.</span></span> <span data-ttu-id="39d32-179">Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="39d32-179">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="39d32-180">tooconfigure egyszeri bejelentkezést a **Cerner központi** oldalsó toosend hello kell **metaadatainak URL-CÍMÉT** túl[Cerner központi támogatási](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="39d32-180">tooconfigure single sign-on on **Cerner Central** side, you need toosend hello **Metadata URL** too[Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="39d32-181">Az alkalmazás ügyféloldali toocomplete hello integrációs konfigurálják hello egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="39d32-181">They configure hello SSO on application side toocomplete hello integration.</span></span>

> [!TIP]
> <span data-ttu-id="39d32-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="39d32-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="39d32-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="39d32-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="39d32-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="39d32-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="39d32-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="39d32-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="39d32-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="39d32-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span> 

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="39d32-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="39d32-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="39d32-189">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="39d32-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="39d32-191">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="39d32-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="39d32-193">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="39d32-193">tooopen hello **User** dialog, click **Add**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="39d32-195">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39d32-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="39d32-197">a.</span><span class="sxs-lookup"><span data-stu-id="39d32-197">a.</span></span> <span data-ttu-id="39d32-198">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="39d32-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="39d32-199">b.</span><span class="sxs-lookup"><span data-stu-id="39d32-199">b.</span></span> <span data-ttu-id="39d32-200">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39d32-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="39d32-201">c.</span><span class="sxs-lookup"><span data-stu-id="39d32-201">c.</span></span> <span data-ttu-id="39d32-202">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="39d32-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="39d32-203">d.</span><span class="sxs-lookup"><span data-stu-id="39d32-203">d.</span></span> <span data-ttu-id="39d32-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="39d32-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="39d32-205">Cerner központi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="39d32-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="39d32-206">**Cerner központi** alkalmazás lehetővé teszi, hogy a hitelesítés bármely összevont identitás-szolgáltatójából.</span><span class="sxs-lookup"><span data-stu-id="39d32-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="39d32-207">Ha a felhasználó képes toolog a toohello alkalmazás kezdőlapját, amelyek összevont, és nincs szükségük a manuális létesítési.</span><span class="sxs-lookup"><span data-stu-id="39d32-207">If a user is able toolog in toohello application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="39d32-208">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="39d32-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="39d32-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezéshez központi hozzáférési tooCerner megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="39d32-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCerner Central.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="39d32-211">**tooassign Britta Simon tooCerner központi, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="39d32-211">**tooassign Britta Simon tooCerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="39d32-212">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="39d32-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="39d32-214">Hello alkalmazások listában válassza ki a **Cerner központi**.</span><span class="sxs-lookup"><span data-stu-id="39d32-214">In hello applications list, select **Cerner Central**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="39d32-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="39d32-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="39d32-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="39d32-218">Click **Add** button.</span></span> <span data-ttu-id="39d32-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39d32-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="39d32-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="39d32-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="39d32-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39d32-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="39d32-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39d32-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="39d32-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="39d32-224">Testing single sign-on</span></span>

<span data-ttu-id="39d32-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="39d32-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="39d32-226">Hello Cerner központi hozzáférési Panel hello csempére kattintva kapja meg automatikusan bejelentkezett tooyour Cerner központi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="39d32-226">When you click hello Cerner Central tile in hello Access Panel, you should get automatically signed-on tooyour Cerner Central application.</span></span> <span data-ttu-id="39d32-227">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="39d32-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39d32-228">További források</span><span class="sxs-lookup"><span data-stu-id="39d32-228">Additional resources</span></span>

* [<span data-ttu-id="39d32-229">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="39d32-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="39d32-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="39d32-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

