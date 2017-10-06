---
title: "Oktatóanyag: Azure Active Directoryval integrált BenefitHub |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BenefitHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: c07d6e44e8cbc79afd79c900664011b059206b56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a><span data-ttu-id="66e33-103">Oktatóanyag: Azure Active Directoryval integrált BenefitHub</span><span class="sxs-lookup"><span data-stu-id="66e33-103">Tutorial: Azure Active Directory integration with BenefitHub</span></span>

<span data-ttu-id="66e33-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BenefitHub az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="66e33-104">In this tutorial, you learn how toointegrate BenefitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66e33-105">BenefitHub integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="66e33-105">Integrating BenefitHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="66e33-106">Megadhatja a hozzáférés tooBenefitHub rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="66e33-106">You can control in Azure AD who has access tooBenefitHub</span></span>
- <span data-ttu-id="66e33-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBenefitHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="66e33-107">You can enable your users tooautomatically get signed-on tooBenefitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66e33-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="66e33-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="66e33-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66e33-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66e33-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66e33-110">Prerequisites</span></span>

<span data-ttu-id="66e33-111">az Azure AD integrálása BenefitHub tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="66e33-111">tooconfigure Azure AD integration with BenefitHub, you need hello following items:</span></span>

- <span data-ttu-id="66e33-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="66e33-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66e33-113">Egy BenefitHub egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="66e33-113">A BenefitHub single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66e33-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="66e33-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66e33-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="66e33-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66e33-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="66e33-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66e33-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66e33-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66e33-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="66e33-118">Scenario description</span></span>
<span data-ttu-id="66e33-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="66e33-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66e33-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="66e33-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66e33-121">Hello gyűjteményből BenefitHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66e33-121">Adding BenefitHub from hello gallery</span></span>
2. <span data-ttu-id="66e33-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66e33-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benefithub-from-hello-gallery"></a><span data-ttu-id="66e33-123">Hello gyűjteményből BenefitHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66e33-123">Adding BenefitHub from hello gallery</span></span>
<span data-ttu-id="66e33-124">tooconfigure hello integrációja BenefitHub az Azure AD-be, meg kell tooadd BenefitHub hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="66e33-124">tooconfigure hello integration of BenefitHub into Azure AD, you need tooadd BenefitHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="66e33-125">**tooadd BenefitHub hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="66e33-125">**tooadd BenefitHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="66e33-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66e33-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66e33-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="66e33-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="66e33-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66e33-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="66e33-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="66e33-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="66e33-133">Hello keresési mezőbe, írja be a **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="66e33-133">In hello search box, type **BenefitHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_search.png)

5. <span data-ttu-id="66e33-135">A hello eredmények panelen válassza ki a **BenefitHub**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="66e33-135">In hello results panel, select **BenefitHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66e33-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66e33-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="66e33-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BenefitHub</span><span class="sxs-lookup"><span data-stu-id="66e33-138">In this section, you configure and test Azure AD single sign-on with BenefitHub based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="66e33-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BenefitHub tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="66e33-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BenefitHub is tooa user in Azure AD.</span></span> <span data-ttu-id="66e33-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BenefitHub közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="66e33-140">In other words, a link relationship between an Azure AD user and hello related user in BenefitHub needs toobe established.</span></span>

<span data-ttu-id="66e33-141">BenefitHub, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="66e33-141">In BenefitHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="66e33-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BenefitHub-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="66e33-142">tooconfigure and test Azure AD single sign-on with BenefitHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="66e33-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="66e33-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="66e33-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66e33-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66e33-145">**[BenefitHub tesztfelhasználó létrehozása](#creating-a-benefithub-test-user)**  -toohave egy megfelelője a Britta Simon a BenefitHub, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="66e33-145">**[Creating a BenefitHub test user](#creating-a-benefithub-test-user)** - toohave a counterpart of Britta Simon in BenefitHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="66e33-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="66e33-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66e33-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="66e33-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66e33-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66e33-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66e33-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BenefitHub alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="66e33-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BenefitHub application.</span></span>

<span data-ttu-id="66e33-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BenefitHub, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="66e33-150">**tooconfigure Azure AD single sign-on with BenefitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="66e33-151">Az Azure portál, a hello hello **BenefitHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="66e33-151">In hello Azure portal, on hello **BenefitHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="66e33-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="66e33-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. <span data-ttu-id="66e33-155">A hello **BenefitHub tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="66e33-155">On hello **BenefitHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_url1.png)
  
    <span data-ttu-id="66e33-157">a.</span><span class="sxs-lookup"><span data-stu-id="66e33-157">a.</span></span> <span data-ttu-id="66e33-158">A hello **azonosító** szövegmező, típus:`urn:benefithub:passport`</span><span class="sxs-lookup"><span data-stu-id="66e33-158">In hello **Identifier** textbox, type: `urn:benefithub:passport`</span></span>
    
    <span data-ttu-id="66e33-159">b.</span><span class="sxs-lookup"><span data-stu-id="66e33-159">b.</span></span> <span data-ttu-id="66e33-160">A hello **válasz URL-CÍMEN** szövegmező, típus:`https://passport.benefithub.info/saml/post/ac`</span><span class="sxs-lookup"><span data-stu-id="66e33-160">In hello **Reply URL** textbox, type: `https://passport.benefithub.info/saml/post/ac`</span></span>

4. <span data-ttu-id="66e33-161">hello BenefitHub alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="66e33-161">hello BenefitHub application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="66e33-162">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="66e33-162">Configure hello following claims for this application.</span></span> <span data-ttu-id="66e33-163">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="66e33-163">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_attribute.png)

5. <span data-ttu-id="66e33-165">A hello **felhasználói attribútumok** hello szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a lemezkép megelőző hello látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="66e33-165">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="66e33-166">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="66e33-166">Attribute Name</span></span> | <span data-ttu-id="66e33-167">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="66e33-167">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="66e33-168">a szervezeti</span><span class="sxs-lookup"><span data-stu-id="66e33-168">organizationid</span></span> | <span data-ttu-id="66e33-169">< a szervezeti ></span><span class="sxs-lookup"><span data-stu-id="66e33-169">< organizationid ></span></span> |

    > [!NOTE]
    > <span data-ttu-id="66e33-170">Az attribútum értéke nem valódi.</span><span class="sxs-lookup"><span data-stu-id="66e33-170">This attribute value is not real.</span></span> <span data-ttu-id="66e33-171">Frissítse a tényleges a szervezeti ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="66e33-171">Update this value with actual organizationid.</span></span> <span data-ttu-id="66e33-172">Ügyfél [BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs) tooget hello tényleges a szervezeti.</span><span class="sxs-lookup"><span data-stu-id="66e33-172">Contact [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) tooget hello actual organizationid.</span></span>
    
    <span data-ttu-id="66e33-173">a.</span><span class="sxs-lookup"><span data-stu-id="66e33-173">a.</span></span> <span data-ttu-id="66e33-174">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66e33-174">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="66e33-177">b.</span><span class="sxs-lookup"><span data-stu-id="66e33-177">b.</span></span> <span data-ttu-id="66e33-178">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="66e33-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="66e33-179">c.</span><span class="sxs-lookup"><span data-stu-id="66e33-179">c.</span></span> <span data-ttu-id="66e33-180">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="66e33-180">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="66e33-181">d.</span><span class="sxs-lookup"><span data-stu-id="66e33-181">d.</span></span> <span data-ttu-id="66e33-182">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="66e33-182">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="66e33-183">Hello SAML-előfeltétel konfigurálása előtt meg kell-e toocontact a [BenefitHub támogatási](https://www.benefithub.com/Home/ContactUs) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="66e33-183">Before you can configure hello SAML assertion, you need toocontact your [BenefitHub support](https://www.benefithub.com/Home/ContactUs) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="66e33-184">Az érték tooconfigure hello egyéni jogcím az alkalmazás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="66e33-184">You need this value tooconfigure hello custom claim for your application.</span></span>

6. <span data-ttu-id="66e33-185">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="66e33-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. <span data-ttu-id="66e33-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="66e33-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="66e33-189">tooconfigure egyszeri bejelentkezést a **BenefitHub** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="66e33-189">tooconfigure single sign-on on **BenefitHub** side, you need toosend hello downloaded **Metadata XML** too[BenefitHub support team](https://www.benefithub.com/Home/ContactUs).</span></span> <span data-ttu-id="66e33-190">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="66e33-190">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="66e33-191">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="66e33-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="66e33-192">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="66e33-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="66e33-193">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66e33-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66e33-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66e33-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="66e33-195">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="66e33-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="66e33-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="66e33-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="66e33-198">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66e33-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66e33-200">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="66e33-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66e33-202">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66e33-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66e33-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="66e33-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66e33-206">a.</span><span class="sxs-lookup"><span data-stu-id="66e33-206">a.</span></span> <span data-ttu-id="66e33-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66e33-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66e33-208">b.</span><span class="sxs-lookup"><span data-stu-id="66e33-208">b.</span></span> <span data-ttu-id="66e33-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="66e33-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="66e33-210">c.</span><span class="sxs-lookup"><span data-stu-id="66e33-210">c.</span></span> <span data-ttu-id="66e33-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="66e33-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="66e33-212">d.</span><span class="sxs-lookup"><span data-stu-id="66e33-212">d.</span></span> <span data-ttu-id="66e33-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="66e33-213">Click **Create**.</span></span>
 
### <a name="creating-a-benefithub-test-user"></a><span data-ttu-id="66e33-214">BenefitHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66e33-214">Creating a BenefitHub test user</span></span>

<span data-ttu-id="66e33-215">Ebben a szakaszban egy BenefitHub Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="66e33-215">In this section, you create a user called Britta Simon in BenefitHub.</span></span> <span data-ttu-id="66e33-216">Együttműködve [BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs) felhasználót is hozzáadhat hello hello BenefitHub platform.</span><span class="sxs-lookup"><span data-stu-id="66e33-216">Work with [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to add hello users in hello BenefitHub platform.</span></span> <span data-ttu-id="66e33-217">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="66e33-217">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="66e33-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="66e33-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="66e33-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBenefitHub megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="66e33-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBenefitHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="66e33-221">**tooassign Britta Simon tooBenefitHub, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="66e33-221">**tooassign Britta Simon tooBenefitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="66e33-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66e33-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="66e33-224">Hello alkalmazások listában válassza ki a **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="66e33-224">In hello applications list, select **BenefitHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_app.png) 

3. <span data-ttu-id="66e33-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="66e33-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="66e33-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="66e33-228">Click **Add** button.</span></span> <span data-ttu-id="66e33-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66e33-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="66e33-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="66e33-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="66e33-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66e33-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66e33-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66e33-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66e33-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="66e33-234">Testing single sign-on</span></span>

<span data-ttu-id="66e33-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="66e33-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="66e33-236">Ha a hozzáférési Panel hello hello BenefitHub csempe gombra kattint, automatikusan bejelentkezett tooyour BenefitHub alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="66e33-236">When you click hello BenefitHub tile in hello Access Panel, you should get automatically signed-on tooyour BenefitHub application.</span></span>
<span data-ttu-id="66e33-237">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="66e33-237">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66e33-238">További források</span><span class="sxs-lookup"><span data-stu-id="66e33-238">Additional resources</span></span>

* [<span data-ttu-id="66e33-239">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="66e33-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66e33-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="66e33-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_203.png

