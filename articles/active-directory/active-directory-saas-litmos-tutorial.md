---
title: "Oktatóanyag: Azure Active Directoryval integrált Litmos |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Litmos között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 026fd10058760f2d63d185ef4aa9d7de3b82525e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a><span data-ttu-id="90981-103">Oktatóanyag: Azure Active Directoryval integrált Litmos</span><span class="sxs-lookup"><span data-stu-id="90981-103">Tutorial: Azure Active Directory integration with Litmos</span></span>

<span data-ttu-id="90981-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Litmos az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="90981-104">In this tutorial, you learn how toointegrate Litmos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="90981-105">Litmos integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="90981-105">Integrating Litmos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="90981-106">Az Azure AD hozzáférési tooLitmos rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="90981-106">You can control in Azure AD who has access tooLitmos.</span></span>
- <span data-ttu-id="90981-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLitmos (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="90981-107">You can enable your users tooautomatically get signed-on tooLitmos (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="90981-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="90981-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="90981-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90981-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90981-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="90981-110">Prerequisites</span></span>

<span data-ttu-id="90981-111">az Azure AD integrálása Litmos tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="90981-111">tooconfigure Azure AD integration with Litmos, you need hello following items:</span></span>

- <span data-ttu-id="90981-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="90981-112">An Azure AD subscription</span></span>
- <span data-ttu-id="90981-113">Egy Litmos egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="90981-113">A Litmos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="90981-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="90981-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="90981-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="90981-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="90981-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="90981-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="90981-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90981-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="90981-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="90981-118">Scenario description</span></span>
<span data-ttu-id="90981-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="90981-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="90981-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="90981-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90981-121">Hello gyűjteményből Litmos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90981-121">Adding Litmos from hello gallery</span></span>
2. <span data-ttu-id="90981-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="90981-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-litmos-from-hello-gallery"></a><span data-ttu-id="90981-123">Hello gyűjteményből Litmos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90981-123">Adding Litmos from hello gallery</span></span>
<span data-ttu-id="90981-124">tooconfigure hello integrációja Litmos az Azure AD-be, meg kell tooadd Litmos hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="90981-124">tooconfigure hello integration of Litmos into Azure AD, you need tooadd Litmos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="90981-125">**tooadd Litmos hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="90981-125">**tooadd Litmos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="90981-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="90981-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="90981-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="90981-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="90981-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="90981-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="90981-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="90981-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="90981-133">Hello keresési mezőbe, írja be a **Litmos**, jelölje be **Litmos** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-133">In hello search box, type **Litmos**, select **Litmos** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Litmos](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="90981-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90981-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="90981-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Litmos.</span><span class="sxs-lookup"><span data-stu-id="90981-136">In this section, you configure and test Azure AD single sign-on with Litmos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="90981-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Litmos tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="90981-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Litmos is tooa user in Azure AD.</span></span> <span data-ttu-id="90981-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Litmos közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="90981-138">In other words, a link relationship between an Azure AD user and hello related user in Litmos needs toobe established.</span></span>

<span data-ttu-id="90981-139">Litmos, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="90981-139">In Litmos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="90981-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Litmos-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="90981-140">tooconfigure and test Azure AD single sign-on with Litmos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="90981-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="90981-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="90981-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90981-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90981-143">**[Litmos tesztfelhasználó létrehozása](#create-a-litmos-test-user)**  -toohave egy megfelelője a Britta Simon a Litmos, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="90981-143">**[Create a Litmos test user](#create-a-litmos-test-user)** - toohave a counterpart of Britta Simon in Litmos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="90981-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="90981-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90981-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="90981-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="90981-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90981-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="90981-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Litmos alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="90981-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Litmos application.</span></span>

<span data-ttu-id="90981-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Litmos, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="90981-148">**tooconfigure Azure AD single sign-on with Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="90981-149">Az Azure portál, a hello hello **Litmos** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="90981-149">In hello Azure portal, on hello **Litmos** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="90981-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="90981-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. <span data-ttu-id="90981-153">A hello **Litmos tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="90981-153">On hello **Litmos Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Litmos tartomány és az URL-címek](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    <span data-ttu-id="90981-155">a.</span><span class="sxs-lookup"><span data-stu-id="90981-155">a.</span></span> <span data-ttu-id="90981-156">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.litmos.com/account/Login`</span><span class="sxs-lookup"><span data-stu-id="90981-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/account/Login`</span></span>

    <span data-ttu-id="90981-157">b.</span><span class="sxs-lookup"><span data-stu-id="90981-157">b.</span></span> <span data-ttu-id="90981-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.litmos.com/integration/samllogin`</span><span class="sxs-lookup"><span data-stu-id="90981-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/integration/samllogin`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="90981-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="90981-159">These values are not real.</span></span> <span data-ttu-id="90981-160">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel, amelyeket az oktatóanyag, vagy forduljon a [Litmos támogatási csoport](https://www.litmos.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="90981-160">Update these values with hello actual Identifier and Reply URL, which are explained later in tutorial or contact [Litmos support team](https://www.litmos.com/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="90981-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="90981-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. <span data-ttu-id="90981-163">Hello konfigurációjának részeként kell toocustomize hello **SAML-jogkivonat attribútumok** Litmos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="90981-163">As part of hello configuration, you need toocustomize hello **SAML Token Attributes** for your Litmos application.</span></span>

    ![Attribútum szakasz](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | <span data-ttu-id="90981-165">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="90981-165">Attribute Name</span></span>   | <span data-ttu-id="90981-166">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="90981-166">Attribute Value</span></span> |   
    | ---------------  | ----------------|
    | <span data-ttu-id="90981-167">Utónév</span><span class="sxs-lookup"><span data-stu-id="90981-167">FirstName</span></span> |<span data-ttu-id="90981-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="90981-168">user.givenname</span></span> |
    | <span data-ttu-id="90981-169">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="90981-169">LastName</span></span>  |<span data-ttu-id="90981-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="90981-170">user.surname</span></span> |
    | <span data-ttu-id="90981-171">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="90981-171">Email</span></span> |<span data-ttu-id="90981-172">User.mail</span><span class="sxs-lookup"><span data-stu-id="90981-172">user.mail</span></span> |

    <span data-ttu-id="90981-173">a.</span><span class="sxs-lookup"><span data-stu-id="90981-173">a.</span></span> <span data-ttu-id="90981-174">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90981-174">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Attribútum hozzáadása](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Attribútum Dailog hozzáadása](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="90981-177">b.</span><span class="sxs-lookup"><span data-stu-id="90981-177">b.</span></span> <span data-ttu-id="90981-178">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="90981-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="90981-179">c.</span><span class="sxs-lookup"><span data-stu-id="90981-179">c.</span></span> <span data-ttu-id="90981-180">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="90981-180">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="90981-181">d.</span><span class="sxs-lookup"><span data-stu-id="90981-181">d.</span></span> <span data-ttu-id="90981-182">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-182">Click **Ok**.</span></span>     

6. <span data-ttu-id="90981-183">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-183">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="90981-185">Egy másik böngészőablakban, bejelentkezés tooyour Litmos vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="90981-185">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

8. <span data-ttu-id="90981-186">Hello hello bal oldali navigációs sávon, kattintson **fiókok**.</span><span class="sxs-lookup"><span data-stu-id="90981-186">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Alkalmazás ügyféloldali a fiókok területen][22] 

9. <span data-ttu-id="90981-188">Kattintson a hello **integrációja** fülre.</span><span class="sxs-lookup"><span data-stu-id="90981-188">Click hello **Integrations** tab.</span></span>
   
    ![Integráció lap][23] 

10. <span data-ttu-id="90981-190">A hello **integrációja** lapra, görgessen lefelé, túl**3. fél integrációja**, és kattintson a **SAML 2.0** fülre.</span><span class="sxs-lookup"><span data-stu-id="90981-190">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0 szakasz][24] 

11. <span data-ttu-id="90981-192">A hello értéket másol **SAML-végpont hello litmos érték:** és illessze be hello **válasz URL-CÍMEN** textbox hello **Litmos tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="90981-192">Copy hello value under **hello SAML endpoint for litmos is:** and paste it into hello **Reply URL** textbox in hello **Litmos Domain and URLs** section in Azure portal.</span></span> 
   
    ![SAML-végpont][26] 

12. <span data-ttu-id="90981-194">Az a **Litmos** alkalmazás, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="90981-194">In your **Litmos** application, perform hello following steps:</span></span>
    
     ![Litmos alkalmazás][25] 
     
     <span data-ttu-id="90981-196">a.</span><span class="sxs-lookup"><span data-stu-id="90981-196">a.</span></span> <span data-ttu-id="90981-197">Kattintson a **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="90981-197">Click **Enable SAML**.</span></span>
    
     <span data-ttu-id="90981-198">b.</span><span class="sxs-lookup"><span data-stu-id="90981-198">b.</span></span> <span data-ttu-id="90981-199">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **SAML X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="90981-199">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **SAML X.509 Certificate** textbox.</span></span>
     
     <span data-ttu-id="90981-200">c.</span><span class="sxs-lookup"><span data-stu-id="90981-200">c.</span></span> <span data-ttu-id="90981-201">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="90981-201">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="90981-202">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="90981-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="90981-203">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="90981-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="90981-204">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="90981-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="90981-205">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="90981-205">Create an Azure AD test user</span></span>

<span data-ttu-id="90981-206">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="90981-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="90981-208">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="90981-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="90981-209">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="90981-211">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="90981-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="90981-213">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="90981-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="90981-215">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="90981-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    <span data-ttu-id="90981-217">a.</span><span class="sxs-lookup"><span data-stu-id="90981-217">a.</span></span> <span data-ttu-id="90981-218">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90981-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="90981-219">b.</span><span class="sxs-lookup"><span data-stu-id="90981-219">b.</span></span> <span data-ttu-id="90981-220">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="90981-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="90981-221">c.</span><span class="sxs-lookup"><span data-stu-id="90981-221">c.</span></span> <span data-ttu-id="90981-222">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="90981-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="90981-223">d.</span><span class="sxs-lookup"><span data-stu-id="90981-223">d.</span></span> <span data-ttu-id="90981-224">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-224">Click **Create**.</span></span>
  
### <a name="create-a-litmos-test-user"></a><span data-ttu-id="90981-225">Litmos tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="90981-225">Create a Litmos test user</span></span>

<span data-ttu-id="90981-226">hello ebben a szakaszban célja toocreate Litmos Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="90981-226">hello objective of this section is toocreate a user called Britta Simon in Litmos.</span></span>  
<span data-ttu-id="90981-227">hello Litmos alkalmazás támogatja a csak időponthoz kötött kiépítés.</span><span class="sxs-lookup"><span data-stu-id="90981-227">hello Litmos application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="90981-228">Ez azt jelenti, egy felhasználói fiók automatikusan létrejön szükség kísérlet tooaccess hello használata az alkalmazások hello hozzáférési Panel alatt.</span><span class="sxs-lookup"><span data-stu-id="90981-228">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

<span data-ttu-id="90981-229">**toocreate Britta Simon meghívta Litmos, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="90981-229">**toocreate a user called Britta Simon in Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="90981-230">Egy másik böngészőablakban, bejelentkezés tooyour Litmos vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="90981-230">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

2. <span data-ttu-id="90981-231">Hello hello bal oldali navigációs sávon, kattintson **fiókok**.</span><span class="sxs-lookup"><span data-stu-id="90981-231">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Alkalmazás ügyféloldali a fiókok területen][22] 

3. <span data-ttu-id="90981-233">Kattintson a hello **integrációja** fülre.</span><span class="sxs-lookup"><span data-stu-id="90981-233">Click hello **Integrations** tab.</span></span>
   
    ![Integrációk lap][23] 

4. <span data-ttu-id="90981-235">A hello **integrációja** lapra, görgessen lefelé, túl**3. fél integrációja**, és kattintson a **SAML 2.0** fülre.</span><span class="sxs-lookup"><span data-stu-id="90981-235">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0][24] 
    
5. <span data-ttu-id="90981-237">Válassza ki **Autogenerate felhasználók**</span><span class="sxs-lookup"><span data-stu-id="90981-237">Select **Autogenerate Users**</span></span>
   
    ![Felhasználók automatikus létrehozása][27] 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="90981-239">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="90981-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="90981-240">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLitmos megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="90981-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLitmos.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="90981-242">**tooassign Britta Simon tooLitmos, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="90981-242">**tooassign Britta Simon tooLitmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="90981-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="90981-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="90981-245">Hello alkalmazások listában válassza ki a **Litmos**.</span><span class="sxs-lookup"><span data-stu-id="90981-245">In hello applications list, select **Litmos**.</span></span>

    ![hello Litmos hivatkozásra hello alkalmazások listája](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. <span data-ttu-id="90981-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="90981-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="90981-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="90981-249">Click **Add** button.</span></span> <span data-ttu-id="90981-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90981-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="90981-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="90981-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="90981-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90981-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="90981-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90981-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="90981-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="90981-255">Test single sign-on</span></span>

<span data-ttu-id="90981-256">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="90981-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="90981-257">Hello Litmos hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Litmos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="90981-257">When you click hello Litmos tile in hello Access Panel, you should get automatically signed-on tooyour Litmos application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="90981-258">További források</span><span class="sxs-lookup"><span data-stu-id="90981-258">Additional resources</span></span>

* [<span data-ttu-id="90981-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="90981-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90981-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="90981-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

