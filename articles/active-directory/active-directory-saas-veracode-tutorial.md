---
title: "Oktatóanyag: Azure Active Directoryval integrált Veracode |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Veracode között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d17307b3864b7df8ee55f569d8f962e2e315b936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="7d079-103">Oktatóanyag: Azure Active Directoryval integrált Veracode</span><span class="sxs-lookup"><span data-stu-id="7d079-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="7d079-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Veracode az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7d079-104">In this tutorial, you learn how toointegrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7d079-105">Veracode integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-105">Integrating Veracode with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7d079-106">Az Azure AD hozzáférési tooVeracode rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="7d079-106">You can control in Azure AD who has access tooVeracode.</span></span>
- <span data-ttu-id="7d079-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVeracode (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="7d079-107">You can enable your users tooautomatically get signed-on tooVeracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7d079-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="7d079-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="7d079-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7d079-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d079-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d079-110">Prerequisites</span></span>

<span data-ttu-id="7d079-111">az Azure AD integrálása Veracode tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-111">tooconfigure Azure AD integration with Veracode, you need hello following items:</span></span>

- <span data-ttu-id="7d079-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7d079-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7d079-113">Egy Veracode egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7d079-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7d079-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7d079-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7d079-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7d079-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7d079-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7d079-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7d079-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d079-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7d079-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7d079-118">Scenario description</span></span>
<span data-ttu-id="7d079-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7d079-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7d079-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7d079-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7d079-121">Adja hozzá a Veracode hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7d079-121">Add Veracode from hello gallery</span></span>
2. <span data-ttu-id="7d079-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7d079-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-hello-gallery"></a><span data-ttu-id="7d079-123">Adja hozzá a Veracode hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7d079-123">Add Veracode from hello gallery</span></span>
<span data-ttu-id="7d079-124">tooconfigure hello integrációja Veracode az Azure AD-be, meg kell tooadd Veracode hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7d079-124">tooconfigure hello integration of Veracode into Azure AD, you need tooadd Veracode from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7d079-125">**tooadd Veracode hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7d079-125">**tooadd Veracode from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d079-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7d079-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="7d079-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7d079-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7d079-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7d079-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="7d079-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7d079-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="7d079-133">Hello keresési mezőbe, írja be a **Veracode**, jelölje be **Veracode** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-133">In hello search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7d079-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7d079-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7d079-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Veracode.</span><span class="sxs-lookup"><span data-stu-id="7d079-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7d079-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Veracode tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7d079-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veracode is tooa user in Azure AD.</span></span> <span data-ttu-id="7d079-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Veracode közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7d079-138">In other words, a link relationship between an Azure AD user and hello related user in Veracode needs toobe established.</span></span>

<span data-ttu-id="7d079-139">Veracode, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7d079-139">In Veracode, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7d079-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Veracode-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7d079-140">tooconfigure and test Azure AD single sign-on with Veracode, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7d079-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7d079-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7d079-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d079-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7d079-143">**[Veracode tesztfelhasználó létrehozása](#create-a-veracode-test-user)**  -toohave egy megfelelője a Britta Simon a Veracode, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7d079-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - toohave a counterpart of Britta Simon in Veracode that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7d079-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7d079-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7d079-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7d079-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7d079-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7d079-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7d079-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Veracode alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7d079-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="7d079-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Veracode, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7d079-148">**tooconfigure Azure AD single sign-on with Veracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d079-149">Az Azure portál, a hello hello **Veracode** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7d079-149">In hello Azure portal, on hello **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="7d079-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7d079-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="7d079-153">A hello **Veracode tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="7d079-153">On hello **Veracode Domain and URLs** section, the user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="7d079-155">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7d079-155">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="7d079-157">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooVeracode fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="7d079-157">hello objective of this section is toooutline how tooenable users tooauthenticate tooVeracode with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="7d079-158">A Veracode alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **saml-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="7d079-158">Your Veracode application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> <span data-ttu-id="7d079-159">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="7d079-159">hello following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="7d079-160">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="7d079-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="7d079-161">tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-161">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="7d079-162">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="7d079-162">Attribute Name</span></span> | <span data-ttu-id="7d079-163">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="7d079-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="7d079-164">Utónév</span><span class="sxs-lookup"><span data-stu-id="7d079-164">firstname</span></span> |<span data-ttu-id="7d079-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="7d079-165">User.givenname</span></span> |
    | <span data-ttu-id="7d079-166">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="7d079-166">lastname</span></span> |<span data-ttu-id="7d079-167">User.surname</span><span class="sxs-lookup"><span data-stu-id="7d079-167">User.surname</span></span> |
    | <span data-ttu-id="7d079-168">E-mailek</span><span class="sxs-lookup"><span data-stu-id="7d079-168">email</span></span> |<span data-ttu-id="7d079-169">User.mail</span><span class="sxs-lookup"><span data-stu-id="7d079-169">User.mail</span></span> |
    
    <span data-ttu-id="7d079-170">a.</span><span class="sxs-lookup"><span data-stu-id="7d079-170">a.</span></span> <span data-ttu-id="7d079-171">Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="7d079-171">For each data row in hello table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="7d079-172">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="7d079-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="7d079-173">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="7d079-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="7d079-174">b.</span><span class="sxs-lookup"><span data-stu-id="7d079-174">b.</span></span> <span data-ttu-id="7d079-175">A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="7d079-175">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="7d079-176">c.</span><span class="sxs-lookup"><span data-stu-id="7d079-176">c.</span></span> <span data-ttu-id="7d079-177">A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="7d079-177">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="7d079-178">d.</span><span class="sxs-lookup"><span data-stu-id="7d079-178">d.</span></span> <span data-ttu-id="7d079-179">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-179">Click **Ok**.</span></span>

7. <span data-ttu-id="7d079-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7d079-182">A hello **Veracode konfigurációs** kattintson **konfigurálása Veracode** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7d079-182">On hello **Veracode Configuration** section, click **Configure Veracode** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7d079-183">Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7d079-183">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Veracode konfiguráció](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="7d079-185">Egy másik webes böngészőablakban jelentkezzen be a Veracode vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7d079-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="7d079-186">Hello hello felső menüben kattintson a **beállítások**, és kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7d079-186">In hello menu on hello top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="7d079-187">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802911.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="7d079-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="7d079-188">Kattintson a hello **SAML** fülre.</span><span class="sxs-lookup"><span data-stu-id="7d079-188">Click hello **SAML** tab.</span></span>

12. <span data-ttu-id="7d079-189">A hello **szervezeti SAML beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-189">In hello **Organization SAML Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7d079-190">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802912.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="7d079-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="7d079-191">a.</span><span class="sxs-lookup"><span data-stu-id="7d079-191">a.</span></span>  <span data-ttu-id="7d079-192">A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="7d079-192">In  **Issuer** textbox, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="7d079-193">b.</span><span class="sxs-lookup"><span data-stu-id="7d079-193">b.</span></span> <span data-ttu-id="7d079-194">tooupload a letöltött tanúsítvány Azure-portálon, kattintson a **Choose File**.</span><span class="sxs-lookup"><span data-stu-id="7d079-194">tooupload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="7d079-195">c.</span><span class="sxs-lookup"><span data-stu-id="7d079-195">c.</span></span> <span data-ttu-id="7d079-196">Válassza ki **engedélyezze az önkiszolgáló regisztrációt**.</span><span class="sxs-lookup"><span data-stu-id="7d079-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="7d079-197">A hello **önkiszolgáló regisztrációs beállítások** szakaszt, hajtsa végre az alábbi lépésekkel hello, és kattintson a **mentése**:</span><span class="sxs-lookup"><span data-stu-id="7d079-197">In hello **Self Registration Settings** section, perform hello following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="7d079-198">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802913.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="7d079-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="7d079-199">a.</span><span class="sxs-lookup"><span data-stu-id="7d079-199">a.</span></span> <span data-ttu-id="7d079-200">Mint **új felhasználó aktiválása**, jelölje be **nem aktiválási szükséges**.</span><span class="sxs-lookup"><span data-stu-id="7d079-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="7d079-201">b.</span><span class="sxs-lookup"><span data-stu-id="7d079-201">b.</span></span> <span data-ttu-id="7d079-202">Mint **felhasználói adatfrissítések**, jelölje be **preferencia Veracode felhasználói adatok**.</span><span class="sxs-lookup"><span data-stu-id="7d079-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="7d079-203">c.</span><span class="sxs-lookup"><span data-stu-id="7d079-203">c.</span></span> <span data-ttu-id="7d079-204">A **SAML attribútum adatai**, válassza ki a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-204">For **SAML Attribute Details**, select hello following:</span></span>
      * <span data-ttu-id="7d079-205">**Felhasználói szerepkörök**</span><span class="sxs-lookup"><span data-stu-id="7d079-205">**User Roles**</span></span>
      * <span data-ttu-id="7d079-206">**Csoportházirendet felügyelő rendszergazda**</span><span class="sxs-lookup"><span data-stu-id="7d079-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="7d079-207">**Felülvizsgáló**</span><span class="sxs-lookup"><span data-stu-id="7d079-207">**Reviewer**</span></span>
      * <span data-ttu-id="7d079-208">**Biztonsági vezető**</span><span class="sxs-lookup"><span data-stu-id="7d079-208">**Security Lead**</span></span>
      * <span data-ttu-id="7d079-209">**Vezetői**</span><span class="sxs-lookup"><span data-stu-id="7d079-209">**Executive**</span></span>
      * <span data-ttu-id="7d079-210">**Küldő**</span><span class="sxs-lookup"><span data-stu-id="7d079-210">**Submitter**</span></span>
      * <span data-ttu-id="7d079-211">**Létrehozó**</span><span class="sxs-lookup"><span data-stu-id="7d079-211">**Creator**</span></span>
      * <span data-ttu-id="7d079-212">**Az ellenőrzési típusok**</span><span class="sxs-lookup"><span data-stu-id="7d079-212">**All Scan Types**</span></span>
      * <span data-ttu-id="7d079-213">**A csoporttagságot**</span><span class="sxs-lookup"><span data-stu-id="7d079-213">**Team Memberships**</span></span>
      * <span data-ttu-id="7d079-214">**Alapértelmezett csoport**</span><span class="sxs-lookup"><span data-stu-id="7d079-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="7d079-215">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7d079-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7d079-216">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7d079-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7d079-217">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7d079-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7d079-218">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="7d079-218">Create an Azure AD test user</span></span>

<span data-ttu-id="7d079-219">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7d079-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="7d079-221">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7d079-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d079-222">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-222">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7d079-224">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7d079-224">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7d079-226">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7d079-226">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7d079-228">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7d079-228">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7d079-230">a.</span><span class="sxs-lookup"><span data-stu-id="7d079-230">a.</span></span> <span data-ttu-id="7d079-231">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7d079-231">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7d079-232">b.</span><span class="sxs-lookup"><span data-stu-id="7d079-232">b.</span></span> <span data-ttu-id="7d079-233">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="7d079-233">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="7d079-234">c.</span><span class="sxs-lookup"><span data-stu-id="7d079-234">c.</span></span> <span data-ttu-id="7d079-235">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="7d079-235">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="7d079-236">d.</span><span class="sxs-lookup"><span data-stu-id="7d079-236">d.</span></span> <span data-ttu-id="7d079-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="7d079-238">Veracode tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d079-238">Create a Veracode test user</span></span>
<span data-ttu-id="7d079-239">A sorrend tooenable az Azure AD felhasználók toolog Veracode be azok ki kell építenie Veracode be.</span><span class="sxs-lookup"><span data-stu-id="7d079-239">In order tooenable Azure AD users toolog into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="7d079-240">Veracode hello esetben egy automatizált feladat.</span><span class="sxs-lookup"><span data-stu-id="7d079-240">In hello case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="7d079-241">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="7d079-241">There is no action item for you.</span></span> <span data-ttu-id="7d079-242">Felhasználók automatikusan létrejönnek szükség hello első egyszeri bejelentkezési kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="7d079-242">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="7d079-243">Bármely más Veracode felhasználói fiók létrehozása eszközök vagy Veracode tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="7d079-243">You can use any other Veracode user account creation tools or APIs provided by Veracode tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="7d079-244">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="7d079-244">Assign hello Azure AD test user</span></span>

<span data-ttu-id="7d079-245">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooVeracode megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7d079-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeracode.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="7d079-247">**tooassign Britta Simon tooVeracode, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7d079-247">**tooassign Britta Simon tooVeracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d079-248">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7d079-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7d079-250">Hello alkalmazások listában válassza ki a **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="7d079-250">In hello applications list, select **Veracode**.</span></span>

    ![hello Veracode hivatkozásra hello alkalmazások listája](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="7d079-252">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7d079-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="7d079-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d079-254">Click **Add** button.</span></span> <span data-ttu-id="7d079-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7d079-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="7d079-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7d079-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7d079-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7d079-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7d079-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7d079-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7d079-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7d079-260">Test single sign-on</span></span>

<span data-ttu-id="7d079-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7d079-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7d079-262">Ha a hozzáférési Panel hello hello Veracode csempe gombra kattint, automatikusan bejelentkezett tooyour Veracode alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7d079-262">When you click hello Veracode tile in hello Access Panel, you should get automatically signed-on tooyour Veracode application.</span></span>
<span data-ttu-id="7d079-263">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7d079-263">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7d079-264">További források</span><span class="sxs-lookup"><span data-stu-id="7d079-264">Additional resources</span></span>

* [<span data-ttu-id="7d079-265">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7d079-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d079-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7d079-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

