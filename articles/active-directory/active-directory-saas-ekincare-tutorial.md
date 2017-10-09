---
title: "Oktatóanyag: Azure Active Directoryval integrált eKincare |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és eKincare között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 16129e3384132bb34744aadf088bb65f07ed7a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="bffd8-103">Oktatóanyag: Azure Active Directoryval integrált eKincare</span><span class="sxs-lookup"><span data-stu-id="bffd8-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="bffd8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate eKincare az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bffd8-104">In this tutorial, you learn how toointegrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bffd8-105">EKincare integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bffd8-105">Integrating eKincare with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bffd8-106">Megadhatja a hozzáférés tooeKincare rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bffd8-106">You can control in Azure AD who has access tooeKincare</span></span>
- <span data-ttu-id="bffd8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooeKincare (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bffd8-107">You can enable your users tooautomatically get signed-on tooeKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bffd8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bffd8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bffd8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bffd8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bffd8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bffd8-110">Prerequisites</span></span>

<span data-ttu-id="bffd8-111">az Azure AD integrálása eKincare tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bffd8-111">tooconfigure Azure AD integration with eKincare, you need hello following items:</span></span>

- <span data-ttu-id="bffd8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bffd8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bffd8-113">Egy eKincare egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="bffd8-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bffd8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bffd8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bffd8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bffd8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bffd8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bffd8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bffd8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bffd8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bffd8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bffd8-118">Scenario description</span></span>
<span data-ttu-id="bffd8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bffd8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bffd8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bffd8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bffd8-121">Hello gyűjteményből eKincare hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bffd8-121">Adding eKincare from hello gallery</span></span>
2. <span data-ttu-id="bffd8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bffd8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-hello-gallery"></a><span data-ttu-id="bffd8-123">Hello gyűjteményből eKincare hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bffd8-123">Adding eKincare from hello gallery</span></span>
<span data-ttu-id="bffd8-124">tooconfigure hello integrációja eKincare az Azure AD-be, meg kell tooadd eKincare hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bffd8-124">tooconfigure hello integration of eKincare into Azure AD, you need tooadd eKincare from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bffd8-125">**tooadd eKincare hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bffd8-125">**tooadd eKincare from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bffd8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bffd8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bffd8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bffd8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bffd8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bffd8-133">Hello keresési mezőbe, írja be a **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-133">In hello search box, type **eKincare**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="bffd8-135">A hello eredmények panelen válassza ki a **eKincare**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-135">In hello results panel, select **eKincare**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bffd8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bffd8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bffd8-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján eKincare</span><span class="sxs-lookup"><span data-stu-id="bffd8-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bffd8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó eKincare tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bffd8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in eKincare is tooa user in Azure AD.</span></span> <span data-ttu-id="bffd8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello eKincare közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bffd8-140">In other words, a link relationship between an Azure AD user and hello related user in eKincare needs toobe established.</span></span>

<span data-ttu-id="bffd8-141">EKincare, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bffd8-141">In eKincare, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bffd8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés eKincare-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bffd8-142">tooconfigure and test Azure AD single sign-on with eKincare, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bffd8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bffd8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bffd8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bffd8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bffd8-145">**[EKincare tesztfelhasználó létrehozása](#creating-a-ekincare-test-user)**  -toohave Britta Simon egy partner, a eKincare, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bffd8-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - toohave a counterpart of Britta Simon in eKincare that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bffd8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bffd8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bffd8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bffd8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bffd8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bffd8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bffd8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az eKincare alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bffd8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="bffd8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a eKincare, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bffd8-150">**tooconfigure Azure AD single sign-on with eKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="bffd8-151">Az Azure portál, a hello hello **eKincare** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-151">In hello Azure portal, on hello **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bffd8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bffd8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="bffd8-155">A hello **eKincare tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bffd8-155">On hello **eKincare Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="bffd8-157">a.</span><span class="sxs-lookup"><span data-stu-id="bffd8-157">a.</span></span> <span data-ttu-id="bffd8-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="bffd8-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="bffd8-159">b.</span><span class="sxs-lookup"><span data-stu-id="bffd8-159">b.</span></span> <span data-ttu-id="bffd8-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="bffd8-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bffd8-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bffd8-161">These values are not real.</span></span> <span data-ttu-id="bffd8-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="bffd8-163">Ügyfél [eKincare támogatási csoport](mailto:tech@ekincare.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bffd8-163">Contact [eKincare support team](mailto:tech@ekincare.com) tooget these values.</span></span>
 
4. <span data-ttu-id="bffd8-164">eKincare alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="bffd8-164">eKincare application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="bffd8-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bffd8-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="bffd8-166">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="bffd8-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="bffd8-167">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="bffd8-167">hello following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="bffd8-168">hello jogcím neve mindig lesz **"employeeid"** és hello érték, amelynek toouser.extensionattribute1 hello employeeid hello felhasználó tartalmazó hozzárendelt igazolnia.</span><span class="sxs-lookup"><span data-stu-id="bffd8-168">hello claim name will always be **"employeeid"** and hello value of which we have mapped toouser.extensionattribute1, that contains hello employeeid of hello user.</span></span> <span data-ttu-id="bffd8-169">más két jogcím neve Egytényezős hello</span><span class="sxs-lookup"><span data-stu-id="bffd8-169">hello other two claims' name i.e</span></span> <span data-ttu-id="bffd8-170">**"a szervezeti"** és **"szervezetnév"** mindig azonos lesz, és azok értékeit tartalmazza hello hello szervezet hello felhasználó kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="bffd8-170">**"organizationid"** and **"organizationname"** will always be same and their values contain hello details of hello organization of hello user respectively.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="bffd8-172">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bffd8-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="bffd8-173">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="bffd8-173">Attribute Name</span></span> | <span data-ttu-id="bffd8-174">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="bffd8-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="bffd8-175">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="bffd8-175">employeeid</span></span> | <span data-ttu-id="bffd8-176">*User.extensionAttribute1*</span><span class="sxs-lookup"><span data-stu-id="bffd8-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="bffd8-177">a szervezeti</span><span class="sxs-lookup"><span data-stu-id="bffd8-177">organizationid</span></span> | <span data-ttu-id="bffd8-178">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="bffd8-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="bffd8-179">szervezetnév</span><span class="sxs-lookup"><span data-stu-id="bffd8-179">organizationname</span></span> | <span data-ttu-id="bffd8-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="bffd8-180">*user.companyname*</span></span> |

    <span data-ttu-id="bffd8-181">a.</span><span class="sxs-lookup"><span data-stu-id="bffd8-181">a.</span></span> <span data-ttu-id="bffd8-182">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-182">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="bffd8-185">b.</span><span class="sxs-lookup"><span data-stu-id="bffd8-185">b.</span></span> <span data-ttu-id="bffd8-186">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="bffd8-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="bffd8-187">c.</span><span class="sxs-lookup"><span data-stu-id="bffd8-187">c.</span></span> <span data-ttu-id="bffd8-188">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="bffd8-188">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="bffd8-189">d.</span><span class="sxs-lookup"><span data-stu-id="bffd8-189">d.</span></span> <span data-ttu-id="bffd8-190">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="bffd8-190">Click **Ok**</span></span>

6. <span data-ttu-id="bffd8-191">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bffd8-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="bffd8-193">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-193">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="bffd8-195">tooconfigure egyszeri bejelentkezést a **eKincare** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[eKincare támogatási csoport](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="bffd8-195">tooconfigure single sign-on on **eKincare** side, you need toosend hello downloaded **Metadata XML** too[eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="bffd8-196">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="bffd8-196">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="bffd8-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bffd8-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bffd8-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bffd8-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bffd8-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bffd8-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bffd8-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bffd8-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="bffd8-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bffd8-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bffd8-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bffd8-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bffd8-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bffd8-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bffd8-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bffd8-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bffd8-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bffd8-212">a.</span><span class="sxs-lookup"><span data-stu-id="bffd8-212">a.</span></span> <span data-ttu-id="bffd8-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bffd8-214">b.</span><span class="sxs-lookup"><span data-stu-id="bffd8-214">b.</span></span> <span data-ttu-id="bffd8-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bffd8-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bffd8-216">c.</span><span class="sxs-lookup"><span data-stu-id="bffd8-216">c.</span></span> <span data-ttu-id="bffd8-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bffd8-218">d.</span><span class="sxs-lookup"><span data-stu-id="bffd8-218">d.</span></span> <span data-ttu-id="bffd8-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="bffd8-220">EKincare tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bffd8-220">Creating a eKincare test user</span></span>

<span data-ttu-id="bffd8-221">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="bffd8-221">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bffd8-222">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bffd8-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bffd8-223">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooeKincare megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bffd8-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooeKincare.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bffd8-225">**tooassign Britta Simon tooeKincare, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bffd8-225">**tooassign Britta Simon tooeKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="bffd8-226">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bffd8-228">Hello alkalmazások listában válassza ki a **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-228">In hello applications list, select **eKincare**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="bffd8-230">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bffd8-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bffd8-232">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bffd8-232">Click **Add** button.</span></span> <span data-ttu-id="bffd8-233">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bffd8-235">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bffd8-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bffd8-236">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bffd8-237">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bffd8-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bffd8-238">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bffd8-238">Testing single sign-on</span></span>

<span data-ttu-id="bffd8-239">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bffd8-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bffd8-240">Ha a hozzáférési Panel hello hello eKincare csempe gombra kattint, automatikusan bejelentkezett tooyour eKincare alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="bffd8-240">When you click hello eKincare tile in hello Access Panel, you should get automatically signed-on tooyour eKincare application.</span></span>
<span data-ttu-id="bffd8-241">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="bffd8-241">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bffd8-242">További források</span><span class="sxs-lookup"><span data-stu-id="bffd8-242">Additional resources</span></span>

* [<span data-ttu-id="bffd8-243">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bffd8-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bffd8-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bffd8-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

