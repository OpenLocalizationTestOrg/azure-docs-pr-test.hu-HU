---
title: "Oktatóanyag: Azure Active Directoryval integrált FilesAnywhere |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FilesAnywhere között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 376364a5c75f8d069ea6390c58586acb378cd8b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="7a2e9-103">Oktatóanyag: Azure Active Directoryval integrált FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="7a2e9-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="7a2e9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FilesAnywhere az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7a2e9-104">In this tutorial, you learn how toointegrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7a2e9-105">FilesAnywhere integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-105">Integrating FilesAnywhere with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7a2e9-106">Megadhatja a hozzáférés tooFilesAnywhere rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7a2e9-106">You can control in Azure AD who has access tooFilesAnywhere</span></span>
- <span data-ttu-id="7a2e9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFilesAnywhere (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7a2e9-107">You can enable your users tooautomatically get signed-on tooFilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7a2e9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="7a2e9-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="7a2e9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7a2e9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a2e9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7a2e9-110">Prerequisites</span></span>

<span data-ttu-id="7a2e9-111">az Azure AD integrálása FilesAnywhere tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-111">tooconfigure Azure AD integration with FilesAnywhere, you need hello following items:</span></span>

- <span data-ttu-id="7a2e9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7a2e9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7a2e9-113">Egy FilesAnywhere egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7a2e9-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="7a2e9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="7a2e9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7a2e9-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="7a2e9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a2e9-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="7a2e9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-118">Scenario description</span></span>
<span data-ttu-id="7a2e9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7a2e9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7a2e9-121">Hello gyűjteményből FilesAnywhere hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-121">Adding FilesAnywhere from hello gallery</span></span>
2. <span data-ttu-id="7a2e9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7a2e9-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-hello-gallery"></a><span data-ttu-id="7a2e9-123">Hello gyűjteményből FilesAnywhere hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-123">Adding FilesAnywhere from hello gallery</span></span>
<span data-ttu-id="7a2e9-124">tooconfigure hello integrációja FilesAnywhere az Azure AD-be, meg kell tooadd FilesAnywhere hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-124">tooconfigure hello integration of FilesAnywhere into Azure AD, you need tooadd FilesAnywhere from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7a2e9-125">**tooadd FilesAnywhere hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7a2e9-125">**tooadd FilesAnywhere from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a2e9-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7a2e9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7a2e9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7a2e9-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7a2e9-133">Hello keresési mezőbe, írja be a **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-133">In hello search box, type **FilesAnywhere**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="7a2e9-135">A hello eredmények panelen válassza ki a **FilesAnywhere**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-135">In hello results panel, select **FilesAnywhere**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7a2e9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7a2e9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7a2e9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7a2e9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FilesAnywhere tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FilesAnywhere is tooa user in Azure AD.</span></span> <span data-ttu-id="7a2e9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FilesAnywhere közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-140">In other words, a link relationship between an Azure AD user and hello related user in FilesAnywhere needs toobe established.</span></span>

<span data-ttu-id="7a2e9-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="7a2e9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés FilesAnywhere-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-142">tooconfigure and test Azure AD single sign-on with FilesAnywhere, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7a2e9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7a2e9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7a2e9-145">**[FilesAnywhere tesztfelhasználó létrehozása](#creating-a-filesanywhere-test-user)**  -toohave Britta Simon FilesAnywhere, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - toohave a counterpart of Britta Simon in FilesAnywhere that is linked toohello Azure AD representation of her.</span></span>
3. <span data-ttu-id="7a2e9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
4. <span data-ttu-id="7a2e9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7a2e9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7a2e9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az FilesAnywhere alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="7a2e9-150">**az Azure AD tooconfigure egyszeri bejelentkezést a FilesAnywhere, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7a2e9-150">**tooconfigure Azure AD single sign-on with FilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a2e9-151">Hello Azure felügyeleti portálon, a hello **FilesAnywhere** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-151">In hello Azure Management portal, on hello **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7a2e9-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="7a2e9-155">A hello **FilesAnywhere tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-155">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="7a2e9-157">a.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-157">a.</span></span> <span data-ttu-id="7a2e9-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="7a2e9-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="7a2e9-159">Vegye figyelembe, hogy hello érték **215** van egy **clientid** és csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-159">Please note that hello value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="7a2e9-160">Tooreplace van szüksége az hello tényleges clientid értékkel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-160">You need tooreplace it with hello actual clientid value.</span></span>

4. <span data-ttu-id="7a2e9-161">A hello **FilesAnywhere tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-161">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="7a2e9-163">a.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-163">a.</span></span> <span data-ttu-id="7a2e9-164">Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="7a2e9-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="7a2e9-165">b.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-165">b.</span></span> <span data-ttu-id="7a2e9-166">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="7a2e9-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7a2e9-167">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="7a2e9-168">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges URL-cím bejelentkezési és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-168">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="7a2e9-169">Ügyfél [FilesAnywhere támogatási csoport](mailto:support@FilesAnywhere.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) tooget these values.</span></span> 

5. <span data-ttu-id="7a2e9-170">FilesAnywhere alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-170">FilesAnywhere Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="7a2e9-171">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-171">Please configure hello following claims for this application.</span></span> <span data-ttu-id="7a2e9-172">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-172">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="7a2e9-173">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-173">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="7a2e9-175">Amikor a felhasználók jelentkezik be a FilesAnywhere azok hello érték beolvasásához hello **clientid** attribútumot [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="7a2e9-175">When hello users signs up with FilesAnywhere they get hello value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="7a2e9-176">Tooadd hello "Ügyfél-azonosító" attribútum FilesAnywhere által biztosított hello egyedi értékkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-176">You have tooadd hello "Client Id" attribute with hello unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="7a2e9-177">A fent látható összes attribútum megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="7a2e9-178">Vegye figyelembe, hogy hello érték **2331** a **clientid** csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-178">Please note that hello value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="7a2e9-179">Tooprovide hello tényleges érték van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-179">You need tooprovide hello actual value.</span></span>


6. <span data-ttu-id="7a2e9-180">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-180">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="7a2e9-181">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="7a2e9-181">Attribute Name</span></span> | <span data-ttu-id="7a2e9-182">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="7a2e9-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="7a2e9-183">ClientID</span><span class="sxs-lookup"><span data-stu-id="7a2e9-183">clientid</span></span> | <span data-ttu-id="7a2e9-184">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="7a2e9-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="7a2e9-185">a.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-185">a.</span></span> <span data-ttu-id="7a2e9-186">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-186">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="7a2e9-189">b.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-189">b.</span></span> <span data-ttu-id="7a2e9-190">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-190">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="7a2e9-191">c.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-191">c.</span></span> <span data-ttu-id="7a2e9-192">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-192">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="7a2e9-193">d.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-193">d.</span></span> <span data-ttu-id="7a2e9-194">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="7a2e9-194">Click **Ok**</span></span>

7. <span data-ttu-id="7a2e9-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-195">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7a2e9-197">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-197">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="7a2e9-199">A hello **FilesAnywhere konfigurációs** kattintson **konfigurálása FilesAnywhere** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-199">On hello **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** tooopen **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="7a2e9-202">tooget SSO konfigurációs FilesAnywhere végén, lépjen kapcsolatba az alkalmazás teljes [FilesAnywhere támogatási csoport](mailto:support@FilesAnywhere.com) és letöltött hello SAML jogkivonat-aláíró tanúsítvány és az egyszeri bejelentkezés (SSO) URL-címet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-202">tooget SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them hello downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7a2e9-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="7a2e9-204">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-204">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7a2e9-206">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7a2e9-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a2e9-207">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-207">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7a2e9-209">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-209">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7a2e9-211">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-211">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7a2e9-213">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a2e9-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7a2e9-215">a.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-215">a.</span></span> <span data-ttu-id="7a2e9-216">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7a2e9-217">b.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-217">b.</span></span> <span data-ttu-id="7a2e9-218">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7a2e9-219">c.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-219">c.</span></span> <span data-ttu-id="7a2e9-220">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7a2e9-221">d.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-221">d.</span></span> <span data-ttu-id="7a2e9-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="7a2e9-223">FilesAnywhere tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a2e9-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="7a2e9-224">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-224">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7a2e9-225">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7a2e9-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7a2e9-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooFilesAnywhere megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFilesAnywhere.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7a2e9-228">**tooassign Britta Simon tooFilesAnywhere, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7a2e9-228">**tooassign Britta Simon tooFilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a2e9-229">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7a2e9-231">Hello alkalmazások listában válassza ki a **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-231">In hello applications list, select **FilesAnywhere**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="7a2e9-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7a2e9-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-235">Click **Add** button.</span></span> <span data-ttu-id="7a2e9-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7a2e9-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7a2e9-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7a2e9-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="7a2e9-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7a2e9-241">Testing single sign-on</span></span>

<span data-ttu-id="7a2e9-242">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-242">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7a2e9-243">Ha a hozzáférési Panel hello hello FilesAnywhere csempe gombra kattint, automatikusan bejelentkezett tooyour FilesAnywhere alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7a2e9-243">When you click hello FilesAnywhere tile in hello Access Panel, you should get automatically signed-on tooyour FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7a2e9-244">További források</span><span class="sxs-lookup"><span data-stu-id="7a2e9-244">Additional resources</span></span>

* [<span data-ttu-id="7a2e9-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7a2e9-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7a2e9-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7a2e9-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
