---
title: "Oktatóanyag: Azure Active Directoryval integrált DigiCert |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a DigiCert között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 646f3129-aa67-4875-9073-1d0b6a3173d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 861039d00533b3aeb361d04e45c4460c6fc8cef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-digicert"></a><span data-ttu-id="c8036-103">Oktatóanyag: Azure Active Directoryval integrált DigiCert</span><span class="sxs-lookup"><span data-stu-id="c8036-103">Tutorial: Azure Active Directory integration with DigiCert</span></span>

<span data-ttu-id="c8036-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate DigiCert az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8036-104">In this tutorial, you learn how toointegrate DigiCert with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8036-105">DigiCert integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c8036-105">Integrating DigiCert with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c8036-106">Megadhatja a hozzáférés tooDigiCert rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c8036-106">You can control in Azure AD who has access tooDigiCert</span></span>
- <span data-ttu-id="c8036-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDigiCert (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c8036-107">You can enable your users tooautomatically get signed-on tooDigiCert (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8036-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c8036-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c8036-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8036-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8036-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8036-110">Prerequisites</span></span>

<span data-ttu-id="c8036-111">az Azure AD integrálása DigiCert tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="c8036-111">tooconfigure Azure AD integration with DigiCert, you need hello following items:</span></span>

- <span data-ttu-id="c8036-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c8036-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8036-113">A DigiCert egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c8036-113">A DigiCert single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8036-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c8036-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8036-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c8036-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8036-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8036-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8036-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8036-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8036-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c8036-118">Scenario description</span></span>
<span data-ttu-id="c8036-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c8036-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8036-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c8036-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8036-121">Hello gyűjteményből DigiCert hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8036-121">Adding DigiCert from hello gallery</span></span>
2. <span data-ttu-id="c8036-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8036-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-digicert-from-hello-gallery"></a><span data-ttu-id="c8036-123">Hello gyűjteményből DigiCert hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8036-123">Adding DigiCert from hello gallery</span></span>
<span data-ttu-id="c8036-124">tooconfigure hello integrációja DigiCert az Azure AD-be, meg kell tooadd DigiCert hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c8036-124">tooconfigure hello integration of DigiCert into Azure AD, you need tooadd DigiCert from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c8036-125">**tooadd DigiCert hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c8036-125">**tooadd DigiCert from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8036-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8036-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8036-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8036-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c8036-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8036-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c8036-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c8036-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c8036-133">Hello keresési mezőbe, írja be a **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="c8036-133">In hello search box, type **DigiCert**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_search.png)

5. <span data-ttu-id="c8036-135">A hello eredmények panelen válassza ki a **DigiCert**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c8036-135">In hello results panel, select **DigiCert**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8036-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8036-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8036-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a DigiCert "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="c8036-138">In this section, you configure and test Azure AD single sign-on with DigiCert based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c8036-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó DigiCert tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c8036-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DigiCert is tooa user in Azure AD.</span></span> <span data-ttu-id="c8036-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello DigiCert közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c8036-140">In other words, a link relationship between an Azure AD user and hello related user in DigiCert needs toobe established.</span></span>

<span data-ttu-id="c8036-141">A DigiCert, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c8036-141">In DigiCert, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c8036-142">tooconfigure és az Azure AD az egyszeri bejelentkezés DigiCert-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c8036-142">tooconfigure and test Azure AD single sign-on with DigiCert, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c8036-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c8036-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c8036-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8036-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8036-145">**[A DigiCert tesztfelhasználó létrehozása](#creating-a-digicert-test-user)**  -toohave egy megfelelője a Britta Simon a DigiCert, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c8036-145">**[Creating a DigiCert test user](#creating-a-digicert-test-user)** - toohave a counterpart of Britta Simon in DigiCert that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8036-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c8036-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8036-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c8036-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8036-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8036-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8036-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a DigiCert alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="c8036-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DigiCert application.</span></span>

<span data-ttu-id="c8036-150">**az Azure AD tooconfigure egyszeri bejelentkezést a DigiCert, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c8036-150">**tooconfigure Azure AD single sign-on with DigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8036-151">Az Azure portál, a hello hello **DigiCert** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c8036-151">In hello Azure portal, on hello **DigiCert** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c8036-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c8036-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_samlbase.png)

3. <span data-ttu-id="c8036-155">A hello **DigiCert tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="c8036-155">On hello **DigiCert Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_url.png)

4. <span data-ttu-id="c8036-157">DigiCert alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="c8036-157">DigiCert application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c8036-158">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c8036-158">Configure hello following claims for this application.</span></span> <span data-ttu-id="c8036-159">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="c8036-159">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="c8036-160">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c8036-160">hello following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_attributes.png)
    
5. <span data-ttu-id="c8036-162">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c8036-162">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="c8036-163">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c8036-163">Attribute Name</span></span> | <span data-ttu-id="c8036-164">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c8036-164">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="c8036-165">Vállalati</span><span class="sxs-lookup"><span data-stu-id="c8036-165">company</span></span> | <span data-ttu-id="c8036-166">< companycode ></span><span class="sxs-lookup"><span data-stu-id="c8036-166">< companycode ></span></span> |
    | <span data-ttu-id="c8036-167">digicertrole</span><span class="sxs-lookup"><span data-stu-id="c8036-167">digicertrole</span></span> | <span data-ttu-id="c8036-168">CanAccessCertCentral</span><span class="sxs-lookup"><span data-stu-id="c8036-168">CanAccessCertCentral</span></span> |

    > [!Note]
    > <span data-ttu-id="c8036-169">hello értékének **vállalati** attribútum nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c8036-169">hello value of **company** attribute is not real.</span></span> <span data-ttu-id="c8036-170">Ez az érték frissítés tényleges vállalati kóddal.</span><span class="sxs-lookup"><span data-stu-id="c8036-170">Update this value with actual company code.</span></span> <span data-ttu-id="c8036-171">tooget hello értékének **vállalati** névjegy attribútuma [DigiCert támogatási csoport](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="c8036-171">tooget hello value of **company** attribute contact [DigiCert support team](mailto:support@digicert.com).</span></span>

    <span data-ttu-id="c8036-172">a.</span><span class="sxs-lookup"><span data-stu-id="c8036-172">a.</span></span> <span data-ttu-id="c8036-173">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8036-173">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c8036-176">b.</span><span class="sxs-lookup"><span data-stu-id="c8036-176">b.</span></span> <span data-ttu-id="c8036-177">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="c8036-177">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="c8036-178">c.</span><span class="sxs-lookup"><span data-stu-id="c8036-178">c.</span></span> <span data-ttu-id="c8036-179">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="c8036-179">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c8036-180">d.</span><span class="sxs-lookup"><span data-stu-id="c8036-180">d.</span></span> <span data-ttu-id="c8036-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8036-181">Click **Ok**.</span></span> 

6. <span data-ttu-id="c8036-182">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c8036-182">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_certificate.png) 

7. <span data-ttu-id="c8036-184">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8036-184">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c8036-186">tooconfigure egyszeri bejelentkezést a **DigiCert** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[DigiCert támogatási csoport](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="c8036-186">tooconfigure single sign-on on **DigiCert** side, you need toosend hello downloaded **Metadata XML** too[DigiCert support team](mailto:support@digicert.com).</span></span> <span data-ttu-id="c8036-187">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="c8036-187">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c8036-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c8036-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c8036-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c8036-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c8036-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8036-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8036-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8036-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8036-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c8036-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c8036-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c8036-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8036-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8036-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8036-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c8036-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8036-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8036-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8036-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c8036-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8036-203">a.</span><span class="sxs-lookup"><span data-stu-id="c8036-203">a.</span></span> <span data-ttu-id="c8036-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8036-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8036-205">b.</span><span class="sxs-lookup"><span data-stu-id="c8036-205">b.</span></span> <span data-ttu-id="c8036-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8036-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8036-207">c.</span><span class="sxs-lookup"><span data-stu-id="c8036-207">c.</span></span> <span data-ttu-id="c8036-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c8036-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c8036-209">d.</span><span class="sxs-lookup"><span data-stu-id="c8036-209">d.</span></span> <span data-ttu-id="c8036-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8036-210">Click **Create**.</span></span>
 
### <a name="creating-a-digicert-test-user"></a><span data-ttu-id="c8036-211">A DigiCert tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8036-211">Creating a DigiCert test user</span></span>

<span data-ttu-id="c8036-212">Ebben a szakaszban a DigiCert Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c8036-212">In this section, you create a user called Britta Simon in DigiCert.</span></span> <span data-ttu-id="c8036-213">Adjon együttműködve [DigiCert támogatási csoport](mailto:support@digicert.com) DigiCert tooadd hello felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="c8036-213">Please work with [DigiCert support team](mailto:support@digicert.com) tooadd hello users in DigiCert.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c8036-214">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c8036-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c8036-215">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDigiCert megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c8036-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDigiCert.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c8036-217">**tooassign Britta Simon tooDigiCert, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c8036-217">**tooassign Britta Simon tooDigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8036-218">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8036-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c8036-220">Hello alkalmazások listában válassza ki a **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="c8036-220">In hello applications list, select **DigiCert**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_app.png) 

3. <span data-ttu-id="c8036-222">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c8036-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c8036-224">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8036-224">Click **Add** button.</span></span> <span data-ttu-id="c8036-225">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8036-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c8036-227">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c8036-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c8036-228">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8036-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8036-229">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8036-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c8036-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8036-230">Testing single sign-on</span></span>

<span data-ttu-id="c8036-231">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c8036-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c8036-232">Ha a hozzáférési Panel hello hello DigiCert csempe gombra kattint, automatikusan bejelentkezett tooyour DeigiCert alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c8036-232">When you click hello DigiCert tile in hello Access Panel, you should get automatically signed-on tooyour DeigiCert application.</span></span>
<span data-ttu-id="c8036-233">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c8036-233">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c8036-234">További források</span><span class="sxs-lookup"><span data-stu-id="c8036-234">Additional resources</span></span>

* [<span data-ttu-id="c8036-235">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c8036-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8036-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c8036-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_203.png

