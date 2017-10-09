---
title: "Oktatóanyag: Azure Active Directoryval integrált ADP Globalview |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ADP Globalview között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: aee2d56f05b486d12facbc41c9503455094604ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a><span data-ttu-id="39472-103">Oktatóanyag: Azure Active Directoryval integrált ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="39472-103">Tutorial: Azure Active Directory integration with ADP Globalview</span></span>

<span data-ttu-id="39472-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ADP Globalview az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="39472-104">In this tutorial, you learn how toointegrate ADP Globalview with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="39472-105">ADP Globalview integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="39472-105">Integrating ADP Globalview with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="39472-106">Megadhatja a hozzáférés tooADP Globalview rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="39472-106">You can control in Azure AD who has access tooADP Globalview</span></span>
- <span data-ttu-id="39472-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooADP Globalview (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="39472-107">You can enable your users tooautomatically get signed-on tooADP Globalview (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="39472-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="39472-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="39472-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="39472-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39472-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="39472-110">Prerequisites</span></span>

<span data-ttu-id="39472-111">az Azure AD integrálása ADP Globalview tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="39472-111">tooconfigure Azure AD integration with ADP Globalview, you need hello following items:</span></span>

- <span data-ttu-id="39472-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="39472-112">An Azure AD subscription</span></span>
- <span data-ttu-id="39472-113">Egy ADP Globalview egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="39472-113">An ADP Globalview single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="39472-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="39472-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="39472-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="39472-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="39472-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="39472-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="39472-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="39472-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="39472-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="39472-118">Scenario description</span></span>
<span data-ttu-id="39472-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="39472-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="39472-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="39472-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="39472-121">Hello gyűjteményből ADP Globalview hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39472-121">Adding ADP Globalview from hello gallery</span></span>
2. <span data-ttu-id="39472-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="39472-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-globalview-from-hello-gallery"></a><span data-ttu-id="39472-123">Hello gyűjteményből ADP Globalview hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39472-123">Adding ADP Globalview from hello gallery</span></span>
<span data-ttu-id="39472-124">tooconfigure hello integrációja ADP Globalview az Azure AD-be, meg kell tooadd ADP Globalview hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="39472-124">tooconfigure hello integration of ADP Globalview into Azure AD, you need tooadd ADP Globalview from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="39472-125">**tooadd ADP Globalview hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="39472-125">**tooadd ADP Globalview from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="39472-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="39472-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="39472-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="39472-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="39472-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="39472-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="39472-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="39472-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="39472-133">Hello keresési mezőbe, írja be a **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="39472-133">In hello search box, type **ADP Globalview**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. <span data-ttu-id="39472-135">A hello eredmények panelen válassza ki a **ADP Globalview**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="39472-135">In hello results panel, select **ADP Globalview**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="39472-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="39472-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="39472-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a ADP Globalview "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="39472-138">In this section, you configure and test Azure AD single sign-on with ADP Globalview based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="39472-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ADP Globalview tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="39472-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP Globalview is tooa user in Azure AD.</span></span> <span data-ttu-id="39472-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ADP Globalview közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="39472-140">In other words, a link relationship between an Azure AD user and hello related user in ADP Globalview needs toobe established.</span></span>

<span data-ttu-id="39472-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ADP Globalview a.</span><span class="sxs-lookup"><span data-stu-id="39472-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP Globalview.</span></span>

<span data-ttu-id="39472-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ADP Globalview-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="39472-142">tooconfigure and test Azure AD single sign-on with ADP Globalview, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="39472-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="39472-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="39472-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39472-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="39472-145">**[Egy ADP Globalview tesztfelhasználó létrehozása](#creating-an-adp-globalview-test-user)**  -toohave egy megfelelője a Britta Simon a ADP Globalview, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="39472-145">**[Creating an ADP Globalview test user](#creating-an-adp-globalview-test-user)** - toohave a counterpart of Britta Simon in ADP Globalview that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="39472-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="39472-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="39472-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="39472-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="39472-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39472-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="39472-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ADP Globalview alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="39472-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP Globalview application.</span></span>

<span data-ttu-id="39472-150">**tooconfigure az Azure AD egyszeri bejelentkezést a ADP Globalview, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="39472-150">**tooconfigure Azure AD single sign-on with ADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="39472-151">Az Azure portál, a hello hello **ADP Globalview** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="39472-151">In hello Azure portal, on hello **ADP Globalview** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="39472-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="39472-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. <span data-ttu-id="39472-155">A hello **ADP Globalview tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39472-155">On hello **ADP Globalview Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     <span data-ttu-id="39472-157">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://<subdomain>.globalview.adp.com/federate` vagy`https://<subdomain>.globalview.adp.com/federate2`</span><span class="sxs-lookup"><span data-stu-id="39472-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.globalview.adp.com/federate` or `https://<subdomain>.globalview.adp.com/federate2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="39472-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="39472-158">hello value is not real.</span></span> <span data-ttu-id="39472-159">Hello érték frissíteni hello tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="39472-159">Update hello value with hello actual Identifier.</span></span> <span data-ttu-id="39472-160">Ügyfél [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="39472-160">Contact [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooget hello value.</span></span>
 
4. <span data-ttu-id="39472-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="39472-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. <span data-ttu-id="39472-163">hello ADP GlobalView alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="39472-163">hello ADP GlobalView application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> 

6. <span data-ttu-id="39472-164">a következő képernyőkép hello az mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="39472-164">hello following screenshot shows an example for it.</span></span> <span data-ttu-id="39472-165">hello jogcím nevek mindig kell **"PersonImmutableID"** és hello érték, amelynek tooExtensionAttribute2, amely tartalmazza azt leképezett hello EmployeeID hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="39472-165">hello claim names always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2, which contains hello EmployeeID of hello user.</span></span> <span data-ttu-id="39472-166">Itt hello felhasználó leképezése az Azure AD tooADP GlobalView hello EmployeeID történik, de tooa különböző érték is az alkalmazás beállításait is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="39472-166">Here hello user mapping from Azure AD tooADP GlobalView is done on hello EmployeeID but you can map it tooa different value also based on your application settings.</span></span> <span data-ttu-id="39472-167">Hello ADP GlobalView team első toouse hello helyes-e a felhasználó azonosítója dolgozni, és hozzárendelését az egyes hello értékkel **"PersonImmutableID"** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="39472-167">You can work with hello ADP GlobalView team first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span> <span data-ttu-id="39472-168">Hello e-mailek és a UserID jogcím hello képen látható módon is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="39472-168">You can also map hello Email and UserID claim as shown in hello picture.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. <span data-ttu-id="39472-170">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39472-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="39472-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="39472-171">Attribute Name</span></span> | <span data-ttu-id="39472-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="39472-172">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="39472-173">personalimmutableid</span><span class="sxs-lookup"><span data-stu-id="39472-173">personalimmutableid</span></span> | <span data-ttu-id="39472-174">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="39472-174">user.extensionattribute2</span></span> |
    | <span data-ttu-id="39472-175">E-mailek</span><span class="sxs-lookup"><span data-stu-id="39472-175">email</span></span>               | <span data-ttu-id="39472-176">User.mail</span><span class="sxs-lookup"><span data-stu-id="39472-176">user.mail</span></span> |
    | <span data-ttu-id="39472-177">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="39472-177">userid</span></span>              | <span data-ttu-id="39472-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="39472-178">user.userprincipalname</span></span>|
    
    <span data-ttu-id="39472-179">a.</span><span class="sxs-lookup"><span data-stu-id="39472-179">a.</span></span> <span data-ttu-id="39472-180">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39472-180">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="39472-183">b.</span><span class="sxs-lookup"><span data-stu-id="39472-183">b.</span></span> <span data-ttu-id="39472-184">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="39472-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="39472-185">c.</span><span class="sxs-lookup"><span data-stu-id="39472-185">c.</span></span> <span data-ttu-id="39472-186">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="39472-186">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="39472-187">d.</span><span class="sxs-lookup"><span data-stu-id="39472-187">d.</span></span> <span data-ttu-id="39472-188">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="39472-188">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="39472-189">Hello SAML-előfeltétel konfigurálása előtt meg kell-e toocontact a [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="39472-189">Before you can configure hello SAML assertion, you need toocontact your [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="39472-190">Az érték tooconfigure hello egyéni jogcím az alkalmazás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="39472-190">You need this value tooconfigure hello custom claim for your application.</span></span> 

8. <span data-ttu-id="39472-191">A hello **ADP Globalview konfigurációs** területén kattintson **konfigurálása ADP Globalview** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="39472-191">On hello **ADP Globalview Configuration** section, click **Configure ADP Globalview** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="39472-192">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="39472-192">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. <span data-ttu-id="39472-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="39472-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="39472-196">tooconfigure egyszeri bejelentkezést a **ADP Globalview** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="39472-196">tooconfigure single sign-on on **ADP Globalview** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="39472-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="39472-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="39472-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="39472-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="39472-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="39472-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="39472-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="39472-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="39472-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="39472-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="39472-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="39472-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="39472-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="39472-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="39472-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="39472-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="39472-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39472-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="39472-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="39472-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="39472-212">a.</span><span class="sxs-lookup"><span data-stu-id="39472-212">a.</span></span> <span data-ttu-id="39472-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="39472-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="39472-214">b.</span><span class="sxs-lookup"><span data-stu-id="39472-214">b.</span></span> <span data-ttu-id="39472-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="39472-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="39472-216">c.</span><span class="sxs-lookup"><span data-stu-id="39472-216">c.</span></span> <span data-ttu-id="39472-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="39472-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="39472-218">d.</span><span class="sxs-lookup"><span data-stu-id="39472-218">d.</span></span> <span data-ttu-id="39472-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="39472-219">Click **Create**.</span></span>
 
### <a name="creating-an-adp-globalview-test-user"></a><span data-ttu-id="39472-220">Egy ADP Globalview tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="39472-220">Creating an ADP Globalview test user</span></span>

<span data-ttu-id="39472-221">hello ebben a szakaszban célja toocreate ADP GlobalView Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="39472-221">hello objective of this section is toocreate a user called Britta Simon in ADP GlobalView.</span></span> <span data-ttu-id="39472-222">Együttműködve [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) tooadd hello felhasználók hello ADP GlobalView fiók.</span><span class="sxs-lookup"><span data-stu-id="39472-222">Work with [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP GlobalView account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="39472-223">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="39472-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="39472-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooADP Globalview megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="39472-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP Globalview.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="39472-226">**tooassign Britta Simon tooADP Globalview, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="39472-226">**tooassign Britta Simon tooADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="39472-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="39472-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="39472-229">Hello alkalmazások listában válassza ki a **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="39472-229">In hello applications list, select **ADP Globalview**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. <span data-ttu-id="39472-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="39472-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="39472-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="39472-233">Click **Add** button.</span></span> <span data-ttu-id="39472-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39472-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="39472-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="39472-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="39472-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39472-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="39472-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="39472-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="39472-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="39472-239">Testing single sign-on</span></span>

<span data-ttu-id="39472-240">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="39472-240">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="39472-241">Hello ADP GlobalView hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ADP GlobalView alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="39472-241">When you click hello ADP GlobalView tile in hello Access Panel, you should get automatically signed-on tooyour ADP GlobalView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39472-242">További források</span><span class="sxs-lookup"><span data-stu-id="39472-242">Additional resources</span></span>

* [<span data-ttu-id="39472-243">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="39472-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="39472-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="39472-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

