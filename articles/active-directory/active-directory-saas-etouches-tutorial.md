---
title: "Oktatóanyag: Azure Active Directoryval integrált etouches |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és etouches között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5f3ff7550e660b0fc52612140ca55061504b5edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="3b4ad-103">Oktatóanyag: Azure Active Directoryval integrált etouches</span><span class="sxs-lookup"><span data-stu-id="3b4ad-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="3b4ad-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate etouches az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b4ad-104">In this tutorial, you learn how toointegrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b4ad-105">Etouches integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-105">Integrating etouches with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3b4ad-106">Megadhatja a hozzáférés tooetouches rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3b4ad-106">You can control in Azure AD who has access tooetouches</span></span>
- <span data-ttu-id="3b4ad-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooetouches (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3b4ad-107">You can enable your users tooautomatically get signed-on tooetouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b4ad-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3b4ad-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3b4ad-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ad-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b4ad-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3b4ad-110">Prerequisites</span></span>

<span data-ttu-id="3b4ad-111">az Azure AD integrálása etouches tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-111">tooconfigure Azure AD integration with etouches, you need hello following items:</span></span>

- <span data-ttu-id="3b4ad-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3b4ad-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b4ad-113">Egy etouches egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3b4ad-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b4ad-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b4ad-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b4ad-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b4ad-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b4ad-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b4ad-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3b4ad-118">Scenario description</span></span>
<span data-ttu-id="3b4ad-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b4ad-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b4ad-121">Hello gyűjteményből etouches hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3b4ad-121">Adding etouches from hello gallery</span></span>
2. <span data-ttu-id="3b4ad-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3b4ad-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-hello-gallery"></a><span data-ttu-id="3b4ad-123">Hello gyűjteményből etouches hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3b4ad-123">Adding etouches from hello gallery</span></span>
<span data-ttu-id="3b4ad-124">tooconfigure hello integrációja etouches az Azure AD-be, meg kell tooadd etouches hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-124">tooconfigure hello integration of etouches into Azure AD, you need tooadd etouches from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3b4ad-125">**tooadd etouches hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3b4ad-125">**tooadd etouches from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b4ad-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="3b4ad-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3b4ad-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="3b4ad-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="3b4ad-133">Hello keresési mezőbe, írja be a **etouches**, jelölje be **etouches** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-133">In hello search box, type **etouches**, select **etouches** from result panel then click **Add** button tooadd hello application.</span></span>

    ![hello eredménylistában etouches](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3b4ad-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b4ad-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="3b4ad-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján etouches.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3b4ad-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó etouches tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in etouches is tooa user in Azure AD.</span></span> <span data-ttu-id="3b4ad-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello etouches közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-138">In other words, a link relationship between an Azure AD user and hello related user in etouches needs toobe established.</span></span>

<span data-ttu-id="3b4ad-139">Etouches, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-139">In etouches, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3b4ad-140">tooconfigure és az Azure AD az egyszeri bejelentkezés etouches-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-140">tooconfigure and test Azure AD single sign-on with etouches, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3b4ad-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3b4ad-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b4ad-143">**[Hozzon létre egy etouches tesztfelhasználó](#create-an-etouches-test-user)**  -toohave Britta Simon egy partner, a etouches, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-143">**[Create an etouches test user](#create-an-etouches-test-user)** - toohave a counterpart of Britta Simon in etouches that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b4ad-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b4ad-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3b4ad-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b4ad-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3b4ad-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az etouches alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="3b4ad-148">**az Azure AD tooconfigure egyszeri bejelentkezést a etouches, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3b4ad-148">**tooconfigure Azure AD single sign-on with etouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b4ad-149">Az Azure portál, a hello hello **etouches** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-149">In hello Azure portal, on hello **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3b4ad-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="3b4ad-153">A hello **etouches tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-153">On hello **etouches Domain and URLs** section, perform hello following steps:</span></span>

    ![az egyszeri bejelentkezési adatokat etouches tartomány és az URL-címek](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="3b4ad-155">a.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-155">a.</span></span> <span data-ttu-id="3b4ad-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="3b4ad-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="3b4ad-157">b.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-157">b.</span></span> <span data-ttu-id="3b4ad-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="3b4ad-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3b4ad-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-159">These values are not real.</span></span> <span data-ttu-id="3b4ad-160">Hello érték frissíti hello tényleges bejelentkezési URL-cím és azonosítója, amelynek az ismertetése hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-160">You update hello value with hello actual Sign on URL and Identifier, which is explained later in hello tutorial.</span></span>
    > 

4. <span data-ttu-id="3b4ad-161">etouches alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-161">etouches application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="3b4ad-162">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-162">Configure hello following claims for this application.</span></span> <span data-ttu-id="3b4ad-163">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútum** hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-163">You can manage hello values of these attributes from hello **User Attribute** of hello application.</span></span> <span data-ttu-id="3b4ad-164">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-164">hello following screenshot shows an example for this.</span></span> 

    ![Felhasználói attribútum](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="3b4ad-166">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-166">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="3b4ad-167">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="3b4ad-167">Attribute Name</span></span> | <span data-ttu-id="3b4ad-168">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="3b4ad-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="3b4ad-169">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="3b4ad-169">Email</span></span> | <span data-ttu-id="3b4ad-170">User.mail</span><span class="sxs-lookup"><span data-stu-id="3b4ad-170">user.mail</span></span> |    
    
    <span data-ttu-id="3b4ad-171">a.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-171">a.</span></span> <span data-ttu-id="3b4ad-172">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-172">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Attribútum hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attribútum párbeszédpanel hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="3b4ad-175">b.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-175">b.</span></span> <span data-ttu-id="3b4ad-176">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-176">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="3b4ad-177">c.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-177">c.</span></span> <span data-ttu-id="3b4ad-178">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-178">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="3b4ad-179">d.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-179">d.</span></span> <span data-ttu-id="3b4ad-180">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="3b4ad-181">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-181">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="3b4ad-183">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-183">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3b4ad-185">az alkalmazáshoz konfigurált SSO tooget hajtsa végre a lépések hello etouches alkalmazásban hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-185">tooget SSO configured for your application, perform hello following steps in hello etouches application:</span></span> 

    ![etouches konfiguráció](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="3b4ad-187">a.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-187">a.</span></span> <span data-ttu-id="3b4ad-188">Bejelentkezési túl**etouches** alkalmazás hello rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-188">Login too**etouches** application using hello Admin rights.</span></span>
   
    <span data-ttu-id="3b4ad-189">b.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-189">b.</span></span> <span data-ttu-id="3b4ad-190">Nyissa meg toohello **SAML** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-190">Go toohello **SAML** Configuration.</span></span>

    <span data-ttu-id="3b4ad-191">c.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-191">c.</span></span> <span data-ttu-id="3b4ad-192">A hello **általános beállítások** szakasz. a letöltött tanúsítvány megnyitása a Jegyzettömbben tartalom másolása hello Azure-portálon, majd illessze be hello IDP metaadatok mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-192">In hello **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy hello content, and then paste it into hello IDP metadata textbox.</span></span> 

    <span data-ttu-id="3b4ad-193">d.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-193">d.</span></span> <span data-ttu-id="3b4ad-194">Kattintson a hello **mentése & kapcsolat** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-194">Click on hello **Save & Stay** button.</span></span>
  
    <span data-ttu-id="3b4ad-195">e.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-195">e.</span></span> <span data-ttu-id="3b4ad-196">Kattintson a hello **frissítéshez tartozó metaadatokat** hello SAML metaadatszakasz gombjára.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-196">Click on hello **Update Metadata** button in hello SAML Metadata section.</span></span> 

    <span data-ttu-id="3b4ad-197">f.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-197">f.</span></span> <span data-ttu-id="3b4ad-198">A megnyíló lapon hello, és végezze el a SSO.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-198">This opens hello page and perform SSO.</span></span> <span data-ttu-id="3b4ad-199">Egyszer hello SSO működik majd hello felhasználónév állíthat be.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-199">Once hello SSO is working then you can set up hello username.</span></span>

    <span data-ttu-id="3b4ad-200">g.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-200">g.</span></span> <span data-ttu-id="3b4ad-201">Hello a felhasználónév mezőben válassza ki a hello **emailaddress** az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-201">In hello Username field, select hello **emailaddress** as shown in hello image below.</span></span> 

    <span data-ttu-id="3b4ad-202">h.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-202">h.</span></span> <span data-ttu-id="3b4ad-203">Másolás hello **SP Entitásazonosító** értékét, és illessze be hello **azonosító** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-203">Copy hello **SP entity ID** value and paste it into hello **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="3b4ad-204">i.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-204">i.</span></span> <span data-ttu-id="3b4ad-205">Másolás hello **egyszeri bejelentkezési URL-cím / ACS** értékét, és illessze be hello **bejelentkezési URL-cím** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-205">Copy hello **SSO URL / ACS** value and paste it into hello **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="3b4ad-206">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="3b4ad-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3b4ad-207">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3b4ad-208">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b4ad-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3b4ad-209">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="3b4ad-209">Create an Azure AD test user</span></span>
<span data-ttu-id="3b4ad-210">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="3b4ad-212">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="3b4ad-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b4ad-213">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b4ad-215">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b4ad-217">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b4ad-219">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3b4ad-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b4ad-221">a.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-221">a.</span></span> <span data-ttu-id="3b4ad-222">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b4ad-223">b.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-223">b.</span></span> <span data-ttu-id="3b4ad-224">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b4ad-225">c.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-225">c.</span></span> <span data-ttu-id="3b4ad-226">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3b4ad-227">d.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-227">d.</span></span> <span data-ttu-id="3b4ad-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="3b4ad-229">Hozzon létre egy etouches tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="3b4ad-229">Create an etouches test user</span></span>

<span data-ttu-id="3b4ad-230">Ebben a szakaszban egy etouches Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="3b4ad-231">Együttműködve [etouches ügyfél-támogatási csoport](https://www.etouches.com/event-software/support/customer-support/) tooadd hello felhasználók hello etouches platform.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) tooadd hello users in hello etouches platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="3b4ad-232">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="3b4ad-232">Assign hello Azure AD test user</span></span>

<span data-ttu-id="3b4ad-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooetouches megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooetouches.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="3b4ad-235">**tooassign Britta Simon tooetouches, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="3b4ad-235">**tooassign Britta Simon tooetouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b4ad-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3b4ad-238">Hello alkalmazások listában válassza ki a **etouches**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-238">In hello applications list, select **etouches**.</span></span>

    ![hello etouches hivatkozásra hello alkalmazások listája](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="3b4ad-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="3b4ad-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-242">Click **Add** button.</span></span> <span data-ttu-id="3b4ad-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="3b4ad-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3b4ad-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b4ad-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3b4ad-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3b4ad-248">Test single sign-on</span></span>


<span data-ttu-id="3b4ad-249">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-249">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3b4ad-250">Hello etouches hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour etouches alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3b4ad-250">When you click hello etouches tile in hello Access Panel, you should get automatically signed-on tooyour etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b4ad-251">További források</span><span class="sxs-lookup"><span data-stu-id="3b4ad-251">Additional resources</span></span>

* [<span data-ttu-id="3b4ad-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3b4ad-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b4ad-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3b4ad-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

