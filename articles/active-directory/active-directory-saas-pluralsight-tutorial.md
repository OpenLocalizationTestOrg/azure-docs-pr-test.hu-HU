---
title: "Oktatóanyag: Azure Active Directoryval integrált Pluralsight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Pluralsight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8394eed79f21fb889816d8dafe2d71187be72b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a><span data-ttu-id="7aa32-103">Oktatóanyag: Azure Active Directoryval integrált Pluralsight</span><span class="sxs-lookup"><span data-stu-id="7aa32-103">Tutorial: Azure Active Directory integration with Pluralsight</span></span>

<span data-ttu-id="7aa32-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Pluralsight az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7aa32-104">In this tutorial, you learn how toointegrate Pluralsight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7aa32-105">Pluralsight integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7aa32-105">Integrating Pluralsight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7aa32-106">Megadhatja a hozzáférés tooPluralsight rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7aa32-106">You can control in Azure AD who has access tooPluralsight</span></span>
- <span data-ttu-id="7aa32-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPluralsight (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7aa32-107">You can enable your users tooautomatically get signed-on tooPluralsight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7aa32-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7aa32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7aa32-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7aa32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7aa32-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7aa32-110">Prerequisites</span></span>

<span data-ttu-id="7aa32-111">az Azure AD integrálása Pluralsight tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7aa32-111">tooconfigure Azure AD integration with Pluralsight, you need hello following items:</span></span>

- <span data-ttu-id="7aa32-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7aa32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7aa32-113">Egy Pluralsight egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7aa32-113">A Pluralsight single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7aa32-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7aa32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7aa32-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7aa32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7aa32-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7aa32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7aa32-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7aa32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7aa32-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7aa32-118">Scenario description</span></span>
<span data-ttu-id="7aa32-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7aa32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7aa32-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7aa32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7aa32-121">Hello gyűjteményből Pluralsight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7aa32-121">Adding Pluralsight from hello gallery</span></span>
2. <span data-ttu-id="7aa32-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7aa32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pluralsight-from-hello-gallery"></a><span data-ttu-id="7aa32-123">Hello gyűjteményből Pluralsight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7aa32-123">Adding Pluralsight from hello gallery</span></span>
<span data-ttu-id="7aa32-124">tooconfigure hello integrációja Pluralsight az Azure AD-be, meg kell tooadd Pluralsight hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7aa32-124">tooconfigure hello integration of Pluralsight into Azure AD, you need tooadd Pluralsight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7aa32-125">**tooadd Pluralsight hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7aa32-125">**tooadd Pluralsight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7aa32-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7aa32-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7aa32-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7aa32-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7aa32-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7aa32-133">Hello keresési mezőbe, írja be a **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-133">In hello search box, type **Pluralsight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. <span data-ttu-id="7aa32-135">A hello eredmények panelen válassza ki a **Pluralsight**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-135">In hello results panel, select **Pluralsight**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7aa32-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7aa32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7aa32-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="7aa32-138">In this section, you configure and test Azure AD single sign-on with Pluralsight based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7aa32-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Pluralsight tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7aa32-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pluralsight is tooa user in Azure AD.</span></span> <span data-ttu-id="7aa32-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Pluralsight közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7aa32-140">In other words, a link relationship between an Azure AD user and hello related user in Pluralsight needs toobe established.</span></span>

<span data-ttu-id="7aa32-141">Pluralsight, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7aa32-141">In Pluralsight, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7aa32-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Pluralsight-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7aa32-142">tooconfigure and test Azure AD single sign-on with Pluralsight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7aa32-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7aa32-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7aa32-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7aa32-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7aa32-145">**[Pluralsight tesztfelhasználó létrehozása](#creating-a-pluralsight-test-user)**  -toohave egy megfelelője a Britta Simon a Pluralsight, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7aa32-145">**[Creating a Pluralsight test user](#creating-a-pluralsight-test-user)** - toohave a counterpart of Britta Simon in Pluralsight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7aa32-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7aa32-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7aa32-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7aa32-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7aa32-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7aa32-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7aa32-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Pluralsight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7aa32-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pluralsight application.</span></span>

<span data-ttu-id="7aa32-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Pluralsight, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7aa32-150">**tooconfigure Azure AD single sign-on with Pluralsight, perform hello following steps:**</span></span>

1. <span data-ttu-id="7aa32-151">Az Azure portál, a hello hello **Pluralsight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-151">In hello Azure portal, on hello **Pluralsight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7aa32-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7aa32-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. <span data-ttu-id="7aa32-155">A hello **Pluralsight tartomány és az URL-címek** területen hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="7aa32-155">On hello **Pluralsight Domain and URLs** section, perform hello following:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    <span data-ttu-id="7aa32-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instance name>.pluralsight.com/sso/<company name>`</span><span class="sxs-lookup"><span data-stu-id="7aa32-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instance name>.pluralsight.com/sso/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7aa32-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="7aa32-158">This value is not real.</span></span> <span data-ttu-id="7aa32-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="7aa32-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7aa32-160">Ügyfél [Pluralsight ügyfél-támogatási csoport](mailto:support@pluralsight.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="7aa32-160">Contact [Pluralsight Client support team](mailto:support@pluralsight.com) tooget this value.</span></span> 
 


4. <span data-ttu-id="7aa32-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7aa32-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. <span data-ttu-id="7aa32-163">hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés hello Azure-portálon és tooconfigure SSO hello Pluralsight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7aa32-163">hello objective of this section is tooenable Azure AD single sign-on in hello Azure portal and tooconfigure SSO in hello Pluralsight application.</span></span>

    <span data-ttu-id="7aa32-164">hello Pluralsight alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-164">hello Pluralsight application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="7aa32-165">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="7aa32-165">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    ><span data-ttu-id="7aa32-167">Azt is megteheti hello **"Egyedi azonosító"** hello megfelelő értékű attribútumot EmployeeID vagy valami mást, amely megfelel a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="7aa32-167">You can also add hello **"Unique ID"** attribute with hello appropriate value like EmployeeID or something else which suits for your organization.</span></span> <span data-ttu-id="7aa32-168">Vegye figyelembe azt is, hogy ez nem kötelező attribútum hello; azonban hozzáadhatja túl a hello egyedi felhasználó azonosítása.</span><span class="sxs-lookup"><span data-stu-id="7aa32-168">Also note that this is not hello required attribute; however, you can add it too identify hello unique user.</span></span> 

6. <span data-ttu-id="7aa32-169">szükséges tooadd hello **SAML-jogkivonat attribútumok**, hello az alábbi táblázatban szereplő minden egyes sorára, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="7aa32-169">tooadd hello required **SAML token attributes**, for each row shown in hello table below, perform hello following steps:</span></span>
   
   | <span data-ttu-id="7aa32-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="7aa32-170">Attribute Name</span></span> | <span data-ttu-id="7aa32-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="7aa32-171">Attribute Value</span></span> |
   | ---| --- |
   | <span data-ttu-id="7aa32-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="7aa32-172">First Name</span></span> |<span data-ttu-id="7aa32-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="7aa32-173">user.givenname</span></span> |
   | <span data-ttu-id="7aa32-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="7aa32-174">Last Name</span></span> |<span data-ttu-id="7aa32-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="7aa32-175">user.surname</span></span> |
   | <span data-ttu-id="7aa32-176">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="7aa32-176">Email</span></span> |<span data-ttu-id="7aa32-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="7aa32-177">user.mail</span></span> |
   
   <span data-ttu-id="7aa32-178">a.</span><span class="sxs-lookup"><span data-stu-id="7aa32-178">a.</span></span> <span data-ttu-id="7aa32-179">Kattintson a **hozzáadása a felhasználói attribútum** tooopen hello **hozzáadása felhasználói Attribure** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-179">Click **add user attribute** tooopen hello **Add User Attribure** dialog.</span></span>
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   <span data-ttu-id="7aa32-181">b.</span><span class="sxs-lookup"><span data-stu-id="7aa32-181">b.</span></span> <span data-ttu-id="7aa32-182">A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="7aa32-182">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
  
   <span data-ttu-id="7aa32-183">c.</span><span class="sxs-lookup"><span data-stu-id="7aa32-183">c.</span></span> <span data-ttu-id="7aa32-184">A hello **attribútumérték** listájában, az adott sorhoz feltüntetett válassza hello attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="7aa32-184">From hello **Attribute Value** list, select hello attribute value shown for that row.</span></span>
  
   <span data-ttu-id="7aa32-185">d.</span><span class="sxs-lookup"><span data-stu-id="7aa32-185">d.</span></span> <span data-ttu-id="7aa32-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-186">Click **Ok**.</span></span>    

7. <span data-ttu-id="7aa32-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7aa32-189">az alkalmazáshoz konfigurált SSO tooget, forduljon a [Pluralsight szolgáltatások](mailTo:professionalservices@pluralsight.com) vonja össze, és adja meg a hello letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7aa32-189">tooget SSO configured for your application, contact [Pluralsight Professional Services](mailTo:professionalservices@pluralsight.com) team and provide hello downloaded metadata file.</span></span>

> [!TIP]
> <span data-ttu-id="7aa32-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7aa32-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7aa32-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7aa32-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7aa32-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7aa32-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7aa32-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7aa32-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="7aa32-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7aa32-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7aa32-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7aa32-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7aa32-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7aa32-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7aa32-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7aa32-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7aa32-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7aa32-205">a.</span><span class="sxs-lookup"><span data-stu-id="7aa32-205">a.</span></span> <span data-ttu-id="7aa32-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7aa32-207">b.</span><span class="sxs-lookup"><span data-stu-id="7aa32-207">b.</span></span> <span data-ttu-id="7aa32-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7aa32-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7aa32-209">c.</span><span class="sxs-lookup"><span data-stu-id="7aa32-209">c.</span></span> <span data-ttu-id="7aa32-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7aa32-211">d.</span><span class="sxs-lookup"><span data-stu-id="7aa32-211">d.</span></span> <span data-ttu-id="7aa32-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-212">Click **Create**.</span></span>
 
### <a name="creating-a-pluralsight-test-user"></a><span data-ttu-id="7aa32-213">Pluralsight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7aa32-213">Creating a Pluralsight test user</span></span>

<span data-ttu-id="7aa32-214">hello ebben a szakaszban célja toocreate Pluralsight Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7aa32-214">hello objective of this section is toocreate a user called Britta Simon in Pluralsight.</span></span> <span data-ttu-id="7aa32-215">Adjon együttműködve [Pluralsight ügyfél-támogatási csoport](mailto:support@pluralsight.com) tooadd hello felhasználók hello Pluralsight fiók.</span><span class="sxs-lookup"><span data-stu-id="7aa32-215">Please work with [Pluralsight Client support team](mailto:support@pluralsight.com) tooadd hello users in hello Pluralsight account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7aa32-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7aa32-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7aa32-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPluralsight megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7aa32-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPluralsight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7aa32-219">**tooassign Britta Simon tooPluralsight, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7aa32-219">**tooassign Britta Simon tooPluralsight, perform hello following steps:**</span></span>

1. <span data-ttu-id="7aa32-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7aa32-222">Hello alkalmazások listában válassza ki a **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-222">In hello applications list, select **Pluralsight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. <span data-ttu-id="7aa32-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7aa32-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7aa32-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7aa32-226">Click **Add** button.</span></span> <span data-ttu-id="7aa32-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7aa32-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7aa32-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7aa32-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7aa32-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7aa32-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7aa32-232">Testing single sign-on</span></span>

<span data-ttu-id="7aa32-233">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="7aa32-233">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7aa32-234">Ha a hozzáférési Panel hello hello Pluralsight csempe gombra kattint, automatikusan bejelentkezett tooyour Pluralsight alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7aa32-234">When you click hello Pluralsight tile in hello Access Panel, you should get automatically signed-on tooyour Pluralsight application.</span></span> <span data-ttu-id="7aa32-235">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7aa32-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7aa32-236">További források</span><span class="sxs-lookup"><span data-stu-id="7aa32-236">Additional resources</span></span>

* [<span data-ttu-id="7aa32-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7aa32-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7aa32-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7aa32-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

