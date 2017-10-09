---
title: "Oktatóanyag: Azure Active Directoryval integrált ADP eTime |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ADP eTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a5c7d14a18220f8c7a5b14055c30662ecd8f14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="2d1b2-103">Oktatóanyag: Azure Active Directoryval integrált ADP eTime</span><span class="sxs-lookup"><span data-stu-id="2d1b2-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="2d1b2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ADP eTime az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2d1b2-104">In this tutorial, you learn how toointegrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d1b2-105">ADP eTime integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-105">Integrating ADP eTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2d1b2-106">Megadhatja a hozzáférés tooADP eTime rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2d1b2-106">You can control in Azure AD who has access tooADP eTime</span></span>
- <span data-ttu-id="2d1b2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooADP eTime (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2d1b2-107">You can enable your users tooautomatically get signed-on tooADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2d1b2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2d1b2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2d1b2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d1b2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d1b2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2d1b2-110">Prerequisites</span></span>

<span data-ttu-id="2d1b2-111">az Azure AD integrálása ADP eTime tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-111">tooconfigure Azure AD integration with ADP eTime, you need hello following items:</span></span>

- <span data-ttu-id="2d1b2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2d1b2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d1b2-113">Egy ADP eTime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2d1b2-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2d1b2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2d1b2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2d1b2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2d1b2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d1b2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2d1b2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-118">Scenario description</span></span>
<span data-ttu-id="2d1b2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2d1b2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d1b2-121">Hello gyűjteményből ADP eTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-121">Adding ADP eTime from hello gallery</span></span>
2. <span data-ttu-id="2d1b2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2d1b2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-hello-gallery"></a><span data-ttu-id="2d1b2-123">Hello gyűjteményből ADP eTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-123">Adding ADP eTime from hello gallery</span></span>
<span data-ttu-id="2d1b2-124">tooconfigure hello integrációja ADP eTime az Azure AD-be, meg kell tooadd ADP eTime hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-124">tooconfigure hello integration of ADP eTime into Azure AD, you need tooadd ADP eTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2d1b2-125">**tooadd ADP eTime hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2d1b2-125">**tooadd ADP eTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d1b2-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2d1b2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2d1b2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2d1b2-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2d1b2-133">Hello keresési mezőbe, írja be a **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-133">In hello search box, type **ADP eTime**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="2d1b2-135">A hello eredmények panelen válassza ki a **ADP eTime**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-135">In hello results panel, select **ADP eTime**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d1b2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2d1b2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d1b2-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az úgynevezett "Britta Simon." tesztfelhasználó alapján ADP eTime</span><span class="sxs-lookup"><span data-stu-id="2d1b2-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2d1b2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ADP eTime tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP eTime is tooa user in Azure AD.</span></span> <span data-ttu-id="2d1b2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ADP eTime közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-140">In other words, a link relationship between an Azure AD user and hello related user in ADP eTime needs toobe established.</span></span>

<span data-ttu-id="2d1b2-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ADP eTime a.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP eTime.</span></span>

<span data-ttu-id="2d1b2-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ADP eTime-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-142">tooconfigure and test Azure AD single sign-on with ADP eTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2d1b2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2d1b2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d1b2-145">**[Egy ADP eTime tesztfelhasználó létrehozása](#creating-an-adp-etime-test-user)**  -toohave Britta Simon egy partner, a ADP eTime, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - toohave a counterpart of Britta Simon in ADP eTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2d1b2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d1b2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2d1b2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2d1b2-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ADP eTime alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="2d1b2-150">**tooconfigure az Azure AD egyszeri bejelentkezést a ADP eTime, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2d1b2-150">**tooconfigure Azure AD single sign-on with ADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d1b2-151">Az Azure portál, a hello hello **ADP eTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-151">In hello Azure portal, on hello **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2d1b2-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="2d1b2-155">A hello **ADP eTime tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-155">On hello **ADP eTime Domain and URLs** section, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="2d1b2-157">a.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-157">a.</span></span> <span data-ttu-id="2d1b2-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="2d1b2-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="2d1b2-159">b.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-159">b.</span></span> <span data-ttu-id="2d1b2-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="2d1b2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="2d1b2-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-161">These values are not hello real.</span></span> <span data-ttu-id="2d1b2-162">Frissítheti ezeket az értékeket a hello tényleges válasz URL-CÍMEN és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-162">Update these values with hello actual Reply URL and Identifier.</span></span> <span data-ttu-id="2d1b2-163">Ügyfél [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooget these values.</span></span>

4. <span data-ttu-id="2d1b2-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="2d1b2-166">hello ADP eTime alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-166">hello ADP eTime application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="2d1b2-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-167">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="2d1b2-168">hello jogcím neve mindig lesz **"PersonImmutableID"** és hello érték, amelynek tooExtensionAttribute2, amely tartalmazza azt leképezett hello EmployeeID hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-168">hello claim name will always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2 which contains hello EmployeeID of hello user.</span></span> 

    <span data-ttu-id="2d1b2-169">Itt a hello EmployeeID hello felhasználó leképezése az Azure AD tooADP eTime fogja elvégezni, de leképezheti a tooa különböző érték is az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-169">Here hello user mapping from Azure AD tooADP eTime will be done on hello EmployeeID but you can map this tooa different value also based on your application settings.</span></span> <span data-ttu-id="2d1b2-170">Ezért kérjük, munkahelyi rendelkező [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) első toouse hello a helyes azonosító egy olyan felhasználó, és képezze le az hello értéket **"PersonImmutableID"** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="2d1b2-172">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="2d1b2-173">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="2d1b2-173">Attribute Name</span></span> | <span data-ttu-id="2d1b2-174">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="2d1b2-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="2d1b2-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="2d1b2-175">PersonImmutableID</span></span> | <span data-ttu-id="2d1b2-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="2d1b2-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="2d1b2-177">a.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-177">a.</span></span> <span data-ttu-id="2d1b2-178">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-178">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="2d1b2-181">b.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-181">b.</span></span> <span data-ttu-id="2d1b2-182">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-182">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="2d1b2-183">c.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-183">c.</span></span> <span data-ttu-id="2d1b2-184">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-184">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2d1b2-185">d.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-185">d.</span></span> <span data-ttu-id="2d1b2-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2d1b2-187">Hello SAML-előfeltétel konfigurálása előtt meg kell-e toocontact a [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-187">Before you can configure hello SAML assertion, you need toocontact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="2d1b2-188">Az érték tooconfigure hello egyéni jogcím az alkalmazás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-188">You need this value tooconfigure hello custom claim for your application.</span></span> 

7. <span data-ttu-id="2d1b2-189">A hello **ADP eTime konfigurációs** kattintson **konfigurálása ADP eTime** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-189">On hello **ADP eTime Configuration** section, click **Configure ADP eTime** tooopen **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="2d1b2-191">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-191">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="2d1b2-193">tooconfigure egyszeri bejelentkezést a **ADP eTime** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d1b2-193">tooconfigure single sign-on on **ADP eTime** side, you need toosend hello downloaded **Metadata XML** too[ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="2d1b2-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2d1b2-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2d1b2-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2d1b2-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2d1b2-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d1b2-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d1b2-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2d1b2-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2d1b2-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d1b2-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2d1b2-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2d1b2-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2d1b2-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2d1b2-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2d1b2-209">a.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-209">a.</span></span> <span data-ttu-id="2d1b2-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2d1b2-211">b.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-211">b.</span></span> <span data-ttu-id="2d1b2-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2d1b2-213">c.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-213">c.</span></span> <span data-ttu-id="2d1b2-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2d1b2-215">d.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-215">d.</span></span> <span data-ttu-id="2d1b2-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="2d1b2-217">Egy ADP eTime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d1b2-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="2d1b2-218">hello ebben a szakaszban célja toocreate ADP eTime Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-218">hello objective of this section is toocreate a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="2d1b2-219">Együttműködve [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) tooadd hello felhasználók hello ADP eTime fiók.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP eTime account.</span></span> 
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2d1b2-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2d1b2-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2d1b2-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooADP eTime megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP eTime.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2d1b2-223">**tooassign Britta Simon tooADP eTime, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2d1b2-223">**tooassign Britta Simon tooADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d1b2-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2d1b2-226">Hello alkalmazások listában válassza ki a **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-226">In hello applications list, select **ADP eTime**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="2d1b2-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2d1b2-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-230">Click **Add** button.</span></span> <span data-ttu-id="2d1b2-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2d1b2-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2d1b2-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2d1b2-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2d1b2-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2d1b2-236">Testing single sign-on</span></span>

<span data-ttu-id="2d1b2-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2d1b2-238">Hello ADP eTime csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ADP eTime alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="2d1b2-238">When you click hello ADP eTime tile in hello Access Panel, you should get automatically signed-on tooyour ADP eTime application.</span></span>
<span data-ttu-id="2d1b2-239">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2d1b2-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2d1b2-240">További források</span><span class="sxs-lookup"><span data-stu-id="2d1b2-240">Additional resources</span></span>

* [<span data-ttu-id="2d1b2-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2d1b2-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d1b2-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2d1b2-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

