---
title: "Oktatóanyag: Azure Active Directoryval integrált Clever |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Clever között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 24430e1e6c750efa5787561aa151201b1fe7d428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a><span data-ttu-id="fc290-103">Oktatóanyag: Azure Active Directoryval integrált Clever</span><span class="sxs-lookup"><span data-stu-id="fc290-103">Tutorial: Azure Active Directory integration with Clever</span></span>

<span data-ttu-id="fc290-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Clever az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fc290-104">In this tutorial, you learn how toointegrate Clever with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fc290-105">Clever integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-105">Integrating Clever with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fc290-106">Az Azure AD hozzáférési tooClever rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="fc290-106">You can control in Azure AD who has access tooClever.</span></span>
- <span data-ttu-id="fc290-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooClever (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="fc290-107">You can enable your users tooautomatically get signed-on tooClever (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fc290-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="fc290-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="fc290-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fc290-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc290-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fc290-110">Prerequisites</span></span>

<span data-ttu-id="fc290-111">az Azure AD integrálása Clever tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-111">tooconfigure Azure AD integration with Clever, you need hello following items:</span></span>

- <span data-ttu-id="fc290-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fc290-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fc290-113">Egy intelligens egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fc290-113">A Clever single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fc290-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="fc290-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fc290-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="fc290-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fc290-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fc290-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fc290-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fc290-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fc290-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fc290-118">Scenario description</span></span>
<span data-ttu-id="fc290-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fc290-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fc290-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fc290-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fc290-121">Hello gyűjteményből Clever hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fc290-121">Adding Clever from hello gallery</span></span>
2. <span data-ttu-id="fc290-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fc290-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clever-from-hello-gallery"></a><span data-ttu-id="fc290-123">Hello gyűjteményből Clever hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fc290-123">Adding Clever from hello gallery</span></span>
<span data-ttu-id="fc290-124">tooconfigure hello integrációja Clever az Azure AD-be, meg kell tooadd Clever hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="fc290-124">tooconfigure hello integration of Clever into Azure AD, you need tooadd Clever from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fc290-125">**tooadd Clever hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fc290-125">**tooadd Clever from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fc290-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fc290-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="fc290-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fc290-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fc290-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fc290-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="fc290-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="fc290-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="fc290-133">Hello keresési mezőbe, írja be a **Clever**, jelölje be **Clever** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-133">In hello search box, type **Clever**, select **Clever** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Intelligens hello eredmények listájában](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fc290-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fc290-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fc290-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Clever.</span><span class="sxs-lookup"><span data-stu-id="fc290-136">In this section, you configure and test Azure AD single sign-on with Clever based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fc290-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Clever tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="fc290-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Clever is tooa user in Azure AD.</span></span> <span data-ttu-id="fc290-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Clever közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="fc290-138">In other words, a link relationship between an Azure AD user and hello related user in Clever needs toobe established.</span></span>

<span data-ttu-id="fc290-139">Clever, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fc290-139">In Clever, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fc290-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Clever-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="fc290-140">tooconfigure and test Azure AD single sign-on with Clever, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fc290-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fc290-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fc290-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fc290-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fc290-143">**[Intelligens tesztfelhasználó létrehozása](#create-a-clever-test-user)**  -toohave egy megfelelője a Britta Simon a Clever, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="fc290-143">**[Create a Clever test user](#create-a-clever-test-user)** - toohave a counterpart of Britta Simon in Clever that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fc290-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fc290-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fc290-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="fc290-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fc290-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fc290-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fc290-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez intelligens alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="fc290-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Clever application.</span></span>

<span data-ttu-id="fc290-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Clever, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fc290-148">**tooconfigure Azure AD single sign-on with Clever, perform hello following steps:**</span></span>

1. <span data-ttu-id="fc290-149">Az Azure portál, a hello hello **Clever** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fc290-149">In hello Azure portal, on hello **Clever** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="fc290-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fc290-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. <span data-ttu-id="fc290-153">A hello **intelligens tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-153">On hello **Clever Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat intelligens tartomány- és URL-címek](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    <span data-ttu-id="fc290-155">a.</span><span class="sxs-lookup"><span data-stu-id="fc290-155">a.</span></span> <span data-ttu-id="fc290-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://clever.com/in/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="fc290-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://clever.com/in/<companyname>`</span></span>

    <span data-ttu-id="fc290-157">b.</span><span class="sxs-lookup"><span data-stu-id="fc290-157">b.</span></span> <span data-ttu-id="fc290-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://clever.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="fc290-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://clever.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fc290-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fc290-159">These values are not real.</span></span> <span data-ttu-id="fc290-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="fc290-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fc290-161">Ügyfél [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fc290-161">Contact [Clever Client support team](https://clever.com/about/contact/) tooget these values.</span></span>

4. <span data-ttu-id="fc290-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fc290-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-clever-tutorial/tutorial_clever_certificate.png)

5. <span data-ttu-id="fc290-164">hello intelligens alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="fc290-164">hello Clever application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span>

    <span data-ttu-id="fc290-165">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="fc290-165">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png) 

6. <span data-ttu-id="fc290-167">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-167">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="fc290-168">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="fc290-168">Attribute Name</span></span>  | <span data-ttu-id="fc290-169">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="fc290-169">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="fc290-170">clever.Student.credentials.District\_felhasználónév</span><span class="sxs-lookup"><span data-stu-id="fc290-170">clever.student.credentials.district\_username</span></span>  | <span data-ttu-id="fc290-171">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="fc290-171">user.userprincipalname</span></span> |
    | <span data-ttu-id="fc290-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="fc290-172">Firstname</span></span>  | <span data-ttu-id="fc290-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="fc290-173">user.givenname</span></span> |
    | <span data-ttu-id="fc290-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="fc290-174">Lastname</span></span>  | <span data-ttu-id="fc290-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="fc290-175">user.surname</span></span> |    

    <span data-ttu-id="fc290-176">a.</span><span class="sxs-lookup"><span data-stu-id="fc290-176">a.</span></span> <span data-ttu-id="fc290-177">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fc290-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="fc290-180">b.</span><span class="sxs-lookup"><span data-stu-id="fc290-180">b.</span></span> <span data-ttu-id="fc290-181">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="fc290-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="fc290-182">c.</span><span class="sxs-lookup"><span data-stu-id="fc290-182">c.</span></span> <span data-ttu-id="fc290-183">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="fc290-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="fc290-184">d.</span><span class="sxs-lookup"><span data-stu-id="fc290-184">d.</span></span> <span data-ttu-id="fc290-185">Hagyja hello **Namespace** szövegbeviteli mező üres.</span><span class="sxs-lookup"><span data-stu-id="fc290-185">Leave hello **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="fc290-186">d.</span><span class="sxs-lookup"><span data-stu-id="fc290-186">d.</span></span> <span data-ttu-id="fc290-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-187">Click **Ok**.</span></span>     

5. <span data-ttu-id="fc290-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="fc290-190">toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-190">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="fc290-191">a.</span><span class="sxs-lookup"><span data-stu-id="fc290-191">a.</span></span> <span data-ttu-id="fc290-192">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="fc290-192">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_appregistrations.png)
   
    <span data-ttu-id="fc290-194">b.</span><span class="sxs-lookup"><span data-stu-id="fc290-194">b.</span></span> <span data-ttu-id="fc290-195">Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fc290-195">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpointicon.png)

    <span data-ttu-id="fc290-197">c.</span><span class="sxs-lookup"><span data-stu-id="fc290-197">c.</span></span> <span data-ttu-id="fc290-198">Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="fc290-198">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpoint.png)
     
    <span data-ttu-id="fc290-200">d.</span><span class="sxs-lookup"><span data-stu-id="fc290-200">d.</span></span> <span data-ttu-id="fc290-201">Most lépjen tulajdonságlapján toohello **Clever** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="fc290-201">Now go toohello property page of **Clever** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_appid.png)

    <span data-ttu-id="fc290-203">e.</span><span class="sxs-lookup"><span data-stu-id="fc290-203">e.</span></span> <span data-ttu-id="fc290-204">Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="fc290-204">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>   

9. <span data-ttu-id="fc290-205">Egy másik webes böngészőablakban jelentkezzen tooyour intelligens vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fc290-205">In a different web browser window, log in tooyour Clever company site as an administrator.</span></span>

10. <span data-ttu-id="fc290-206">Hello eszköztárában kattintson **azonnali bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="fc290-206">In hello toolbar, click **Instant Login**.</span></span>

    <span data-ttu-id="fc290-207">![Azonnali bejelentkezési](./media/active-directory-saas-clever-tutorial/ic798984.png "azonnali bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="fc290-207">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant Login")</span></span>

11. <span data-ttu-id="fc290-208">A hello **azonnali bejelentkezési** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-208">On hello **Instant Login** page, perform hello following steps:</span></span>
      
      <span data-ttu-id="fc290-209">![Azonnali bejelentkezési](./media/active-directory-saas-clever-tutorial/ic798985.png "azonnali bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="fc290-209">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant Login")</span></span>
      
      <span data-ttu-id="fc290-210">a.</span><span class="sxs-lookup"><span data-stu-id="fc290-210">a.</span></span> <span data-ttu-id="fc290-211">Típus hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="fc290-211">Type hello **Login URL**.</span></span>
      
      >[!NOTE]
      ><span data-ttu-id="fc290-212">Hello **bejelentkezési URL-cím** egy egyéni érték.</span><span class="sxs-lookup"><span data-stu-id="fc290-212">hello **Login URL** is a custom value.</span></span> <span data-ttu-id="fc290-213">Ügyfél [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="fc290-213">Contact [Clever Client support team](https://clever.com/about/contact/) tooget this value.</span></span>
      
      <span data-ttu-id="fc290-214">b.</span><span class="sxs-lookup"><span data-stu-id="fc290-214">b.</span></span> <span data-ttu-id="fc290-215">Mint **Identitásrendszere**, jelölje be **az AD FS**.</span><span class="sxs-lookup"><span data-stu-id="fc290-215">As **Identity System**, select **ADFS**.</span></span>

      <span data-ttu-id="fc290-216">c.</span><span class="sxs-lookup"><span data-stu-id="fc290-216">c.</span></span> <span data-ttu-id="fc290-217">Típus hello **metaadatainak URL-CÍMÉT** a hello **metaadatainak URL-CÍMÉT** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fc290-217">Type hello **Metadata URL** in hello **Metadata URL** textbox.</span></span>
      
      <span data-ttu-id="fc290-218">d.</span><span class="sxs-lookup"><span data-stu-id="fc290-218">d.</span></span> <span data-ttu-id="fc290-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-219">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="fc290-220">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="fc290-220">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fc290-221">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="fc290-221">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fc290-222">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fc290-222">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fc290-223">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fc290-223">Create an Azure AD test user</span></span>

<span data-ttu-id="fc290-224">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="fc290-224">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="fc290-226">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="fc290-226">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fc290-227">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-227">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fc290-229">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fc290-229">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fc290-231">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fc290-231">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fc290-233">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fc290-233">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fc290-235">a.</span><span class="sxs-lookup"><span data-stu-id="fc290-235">a.</span></span> <span data-ttu-id="fc290-236">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fc290-236">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fc290-237">b.</span><span class="sxs-lookup"><span data-stu-id="fc290-237">b.</span></span> <span data-ttu-id="fc290-238">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="fc290-238">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="fc290-239">c.</span><span class="sxs-lookup"><span data-stu-id="fc290-239">c.</span></span> <span data-ttu-id="fc290-240">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fc290-240">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="fc290-241">d.</span><span class="sxs-lookup"><span data-stu-id="fc290-241">d.</span></span> <span data-ttu-id="fc290-242">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-242">Click **Create**.</span></span>
 
### <a name="create-a-clever-test-user"></a><span data-ttu-id="fc290-243">Intelligens tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fc290-243">Create a Clever test user</span></span>

<span data-ttu-id="fc290-244">az Azure AD tooenable felhasználók toolog a tooClever, akkor ki kell építenie Clever be.</span><span class="sxs-lookup"><span data-stu-id="fc290-244">tooenable Azure AD users toolog in tooClever, they must be provisioned into Clever.</span></span>

<span data-ttu-id="fc290-245">Használata esetén Clever, [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) felhasználót is hozzáadhat hello hello intelligens platform.</span><span class="sxs-lookup"><span data-stu-id="fc290-245">In case of Clever, Work with [Clever Client support team](https://clever.com/about/contact/) to add hello users in hello Clever platform.</span></span> <span data-ttu-id="fc290-246">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="fc290-246">Users must be created and activated before you use single sign-on.</span></span> 

>[!NOTE]
><span data-ttu-id="fc290-247">Bármely más intelligens felhasználói fiók létrehozása eszközök, vagy intelligens tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="fc290-247">You can use any other Clever user account creation tools or APIs provided by Clever tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="fc290-248">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="fc290-248">Assign hello Azure AD test user</span></span>

<span data-ttu-id="fc290-249">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooClever megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="fc290-249">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooClever.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="fc290-251">**tooassign Britta Simon tooClever, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="fc290-251">**tooassign Britta Simon tooClever, perform hello following steps:**</span></span>

1. <span data-ttu-id="fc290-252">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fc290-252">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fc290-254">Hello alkalmazások listában válassza ki a **Clever**.</span><span class="sxs-lookup"><span data-stu-id="fc290-254">In hello applications list, select **Clever**.</span></span>

    ![hello Clever hello alkalmazások listáját a hivatkozás](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)  

3. <span data-ttu-id="fc290-256">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fc290-256">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="fc290-258">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fc290-258">Click **Add** button.</span></span> <span data-ttu-id="fc290-259">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fc290-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="fc290-261">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fc290-261">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fc290-262">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fc290-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fc290-263">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fc290-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fc290-264">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fc290-264">Test single sign-on</span></span>

<span data-ttu-id="fc290-265">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="fc290-265">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fc290-266">Hello intelligens csempére kattintva a hozzáférési Panel hello, automatikusan bejelentkezett tooyour intelligens alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="fc290-266">When you click hello Clever tile in hello Access Panel, you should get automatically signed-on tooyour Clever application.</span></span>
<span data-ttu-id="fc290-267">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fc290-267">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fc290-268">További források</span><span class="sxs-lookup"><span data-stu-id="fc290-268">Additional resources</span></span>

* [<span data-ttu-id="fc290-269">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="fc290-269">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fc290-270">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fc290-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

