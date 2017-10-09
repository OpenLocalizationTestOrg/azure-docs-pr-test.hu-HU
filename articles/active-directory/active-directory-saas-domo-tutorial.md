---
title: "Oktatóanyag: Azure Active Directoryval integrált Domo |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Domo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: cc70f8e5013f864d275762bbc1f84bd9677e8c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="363ec-103">Oktatóanyag: Azure Active Directoryval integrált Domo</span><span class="sxs-lookup"><span data-stu-id="363ec-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="363ec-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Domo az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="363ec-104">In this tutorial, you learn how toointegrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="363ec-105">Domo integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="363ec-105">Integrating Domo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="363ec-106">Megadhatja a hozzáférés tooDomo rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="363ec-106">You can control in Azure AD who has access tooDomo</span></span>
- <span data-ttu-id="363ec-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDomo (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="363ec-107">You can enable your users tooautomatically get signed-on tooDomo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="363ec-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="363ec-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="363ec-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="363ec-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="363ec-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="363ec-110">Prerequisites</span></span>

<span data-ttu-id="363ec-111">az Azure AD integrálása Domo tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="363ec-111">tooconfigure Azure AD integration with Domo, you need hello following items:</span></span>

- <span data-ttu-id="363ec-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="363ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="363ec-113">Egy Domo egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="363ec-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="363ec-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="363ec-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="363ec-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="363ec-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="363ec-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="363ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="363ec-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="363ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="363ec-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="363ec-118">Scenario description</span></span>
<span data-ttu-id="363ec-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="363ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="363ec-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="363ec-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="363ec-121">Hello gyűjteményből Domo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="363ec-121">Adding Domo from hello gallery</span></span>
2. <span data-ttu-id="363ec-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="363ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-hello-gallery"></a><span data-ttu-id="363ec-123">Hello gyűjteményből Domo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="363ec-123">Adding Domo from hello gallery</span></span>
<span data-ttu-id="363ec-124">tooconfigure hello integrációja Domo az Azure AD-be, meg kell tooadd Domo hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="363ec-124">tooconfigure hello integration of Domo into Azure AD, you need tooadd Domo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="363ec-125">**tooadd Domo hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="363ec-125">**tooadd Domo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="363ec-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="363ec-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="363ec-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="363ec-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="363ec-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="363ec-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="363ec-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="363ec-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="363ec-133">Hello keresési mezőbe, írja be a **Domo**.</span><span class="sxs-lookup"><span data-stu-id="363ec-133">In hello search box, type **Domo**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="363ec-135">A hello eredmények panelen válassza ki a **Domo**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="363ec-135">In hello results panel, select **Domo**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="363ec-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="363ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="363ec-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Domo</span><span class="sxs-lookup"><span data-stu-id="363ec-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="363ec-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Domo tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="363ec-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Domo is tooa user in Azure AD.</span></span> <span data-ttu-id="363ec-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Domo közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="363ec-140">In other words, a link relationship between an Azure AD user and hello related user in Domo needs toobe established.</span></span>

<span data-ttu-id="363ec-141">Domo, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="363ec-141">In Domo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="363ec-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Domo-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="363ec-142">tooconfigure and test Azure AD single sign-on with Domo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="363ec-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="363ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="363ec-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="363ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="363ec-145">**[Domo tesztfelhasználó létrehozása](#creating-a-domo-test-user)**  -toohave egy megfelelője a Britta Simon a Domo, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="363ec-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - toohave a counterpart of Britta Simon in Domo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="363ec-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="363ec-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="363ec-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="363ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="363ec-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="363ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="363ec-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Domo alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="363ec-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="363ec-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Domo, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="363ec-150">**tooconfigure Azure AD single sign-on with Domo, perform hello following steps:**</span></span>

1. <span data-ttu-id="363ec-151">Az Azure portál, a hello hello **Domo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="363ec-151">In hello Azure portal, on hello **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="363ec-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="363ec-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="363ec-155">A hello **Domo tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="363ec-155">On hello **Domo Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="363ec-157">a.</span><span class="sxs-lookup"><span data-stu-id="363ec-157">a.</span></span> <span data-ttu-id="363ec-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="363ec-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="363ec-159">b.</span><span class="sxs-lookup"><span data-stu-id="363ec-159">b.</span></span> <span data-ttu-id="363ec-160">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="363ec-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="363ec-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="363ec-161">These values are not real.</span></span> <span data-ttu-id="363ec-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="363ec-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="363ec-163">Ügyfél [Domo ügyfél-támogatási csoport](mailto:support@domo.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="363ec-163">Contact [Domo Client support team](mailto:support@domo.com) tooget these values.</span></span>

4. <span data-ttu-id="363ec-164">Domo alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="363ec-164">Domo application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="363ec-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="363ec-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="363ec-166">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="363ec-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="363ec-167">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="363ec-167">hello following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="363ec-169">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="363ec-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="363ec-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="363ec-170">Attribute Name</span></span> | <span data-ttu-id="363ec-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="363ec-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="363ec-172">név</span><span class="sxs-lookup"><span data-stu-id="363ec-172">name</span></span> | <span data-ttu-id="363ec-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="363ec-173">user.displayname</span></span> |
    | <span data-ttu-id="363ec-174">E-mailek</span><span class="sxs-lookup"><span data-stu-id="363ec-174">email</span></span> | <span data-ttu-id="363ec-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="363ec-175">user.mail</span></span> |
    
    <span data-ttu-id="363ec-176">a.</span><span class="sxs-lookup"><span data-stu-id="363ec-176">a.</span></span> <span data-ttu-id="363ec-177">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="363ec-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="363ec-180">b.</span><span class="sxs-lookup"><span data-stu-id="363ec-180">b.</span></span> <span data-ttu-id="363ec-181">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="363ec-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="363ec-182">c.</span><span class="sxs-lookup"><span data-stu-id="363ec-182">c.</span></span> <span data-ttu-id="363ec-183">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="363ec-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="363ec-184">d.</span><span class="sxs-lookup"><span data-stu-id="363ec-184">d.</span></span> <span data-ttu-id="363ec-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="363ec-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="363ec-186">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="363ec-186">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="363ec-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="363ec-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="363ec-190">A hello **Domo konfigurációs** kattintson **konfigurálása Domo** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="363ec-190">On hello **Domo Configuration** section, click **Configure Domo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="363ec-191">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="363ec-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="363ec-193">tooconfigure egyszeri bejelentkezést a **Domo** oldalon kell letöltött toosend hello **tanúsítvány**, **SAML Entitásazonosító**, hello **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** és hello **Sign-Out URL-cím** túl[Domo támogatási csoport](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="363ec-193">tooconfigure single sign-on on **Domo** side, you need toosend hello downloaded **Certificate**, **SAML Entity ID**, hello **SAML Single Sign-On Service URL** and hello **Sign-Out URL** too[Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="363ec-194">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="363ec-194">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="363ec-195">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="363ec-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="363ec-196">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="363ec-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="363ec-197">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="363ec-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="363ec-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="363ec-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="363ec-199">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="363ec-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="363ec-201">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="363ec-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="363ec-202">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="363ec-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="363ec-204">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="363ec-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="363ec-206">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="363ec-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="363ec-208">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="363ec-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="363ec-210">a.</span><span class="sxs-lookup"><span data-stu-id="363ec-210">a.</span></span> <span data-ttu-id="363ec-211">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="363ec-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="363ec-212">b.</span><span class="sxs-lookup"><span data-stu-id="363ec-212">b.</span></span> <span data-ttu-id="363ec-213">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="363ec-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="363ec-214">c.</span><span class="sxs-lookup"><span data-stu-id="363ec-214">c.</span></span> <span data-ttu-id="363ec-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="363ec-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="363ec-216">d.</span><span class="sxs-lookup"><span data-stu-id="363ec-216">d.</span></span> <span data-ttu-id="363ec-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="363ec-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="363ec-218">Domo tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="363ec-218">Creating a Domo test user</span></span>

<span data-ttu-id="363ec-219">hello ebben a szakaszban célja toocreate Domo Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="363ec-219">hello objective of this section is toocreate a user called Britta Simon in Domo.</span></span> <span data-ttu-id="363ec-220">Domo támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="363ec-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="363ec-221">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="363ec-221">There is no action item for you in this section.</span></span> <span data-ttu-id="363ec-222">Új felhasználó jön létre egy kísérlet tooaccess Domo során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="363ec-222">A new user is created during an attempt tooaccess Domo if it doesn't exist yet.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="363ec-223">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="363ec-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="363ec-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDomo megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="363ec-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDomo.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="363ec-226">**tooassign Britta Simon tooDomo, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="363ec-226">**tooassign Britta Simon tooDomo, perform hello following steps:**</span></span>

1. <span data-ttu-id="363ec-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="363ec-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="363ec-229">Hello alkalmazások listában válassza ki a **Domo**.</span><span class="sxs-lookup"><span data-stu-id="363ec-229">In hello applications list, select **Domo**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="363ec-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="363ec-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="363ec-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="363ec-233">Click **Add** button.</span></span> <span data-ttu-id="363ec-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="363ec-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="363ec-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="363ec-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="363ec-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="363ec-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="363ec-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="363ec-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="363ec-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="363ec-239">Testing single sign-on</span></span>

<span data-ttu-id="363ec-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="363ec-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="363ec-241">Ha a hozzáférési Panel hello hello Domo csempe gombra kattint, automatikusan bejelentkezett tooyour Domo alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="363ec-241">When you click hello Domo tile in hello Access Panel, you should get automatically signed-on tooyour Domo application.</span></span>

<span data-ttu-id="363ec-242">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="363ec-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="363ec-243">További források</span><span class="sxs-lookup"><span data-stu-id="363ec-243">Additional resources</span></span>

* [<span data-ttu-id="363ec-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="363ec-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="363ec-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="363ec-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

