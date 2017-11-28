---
title: "Oktatóanyag: Azure Active Directoryval integrált Druva |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Druva között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a><span data-ttu-id="5e40d-103">Oktatóanyag: Azure Active Directoryval integrált Druva</span><span class="sxs-lookup"><span data-stu-id="5e40d-103">Tutorial: Azure Active Directory integration with Druva</span></span>

<span data-ttu-id="5e40d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Druva az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5e40d-104">In this tutorial, you learn how toointegrate Druva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5e40d-105">Druva integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-105">Integrating Druva with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5e40d-106">Az Azure AD hozzáférési tooDruva rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="5e40d-106">You can control in Azure AD who has access tooDruva.</span></span>
- <span data-ttu-id="5e40d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDruva (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="5e40d-107">You can enable your users tooautomatically get signed-on tooDruva (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5e40d-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="5e40d-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5e40d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5e40d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e40d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5e40d-110">Prerequisites</span></span>

<span data-ttu-id="5e40d-111">az Azure AD integrálása Druva tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-111">tooconfigure Azure AD integration with Druva, you need hello following items:</span></span>

- <span data-ttu-id="5e40d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5e40d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5e40d-113">Egy Druva egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5e40d-113">A Druva single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5e40d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5e40d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5e40d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5e40d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5e40d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5e40d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5e40d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e40d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5e40d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5e40d-118">Scenario description</span></span>
<span data-ttu-id="5e40d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5e40d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5e40d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5e40d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5e40d-121">Hello gyűjteményből Druva hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5e40d-121">Adding Druva from hello gallery</span></span>
2. <span data-ttu-id="5e40d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5e40d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-druva-from-hello-gallery"></a><span data-ttu-id="5e40d-123">Hello gyűjteményből Druva hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5e40d-123">Adding Druva from hello gallery</span></span>
<span data-ttu-id="5e40d-124">tooconfigure hello integrációja Druva az Azure AD-be, meg kell tooadd Druva hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5e40d-124">tooconfigure hello integration of Druva into Azure AD, you need tooadd Druva from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5e40d-125">**tooadd Druva hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5e40d-125">**tooadd Druva from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e40d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="5e40d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5e40d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="5e40d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5e40d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="5e40d-133">Hello keresési mezőbe, írja be a **Druva**, jelölje be **Druva** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-133">In hello search box, type **Druva**, select **Druva** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Druva](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5e40d-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e40d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5e40d-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Druva.</span><span class="sxs-lookup"><span data-stu-id="5e40d-136">In this section, you configure and test Azure AD single sign-on with Druva based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5e40d-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Druva tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5e40d-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Druva is tooa user in Azure AD.</span></span> <span data-ttu-id="5e40d-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Druva közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5e40d-138">In other words, a link relationship between an Azure AD user and hello related user in Druva needs toobe established.</span></span>

<span data-ttu-id="5e40d-139">Druva, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5e40d-139">In Druva, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5e40d-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Druva-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5e40d-140">tooconfigure and test Azure AD single sign-on with Druva, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5e40d-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5e40d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5e40d-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e40d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5e40d-143">**[Druva tesztfelhasználó létrehozása](#create-a-druva-test-user)**  -toohave egy megfelelője a Britta Simon a Druva, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5e40d-143">**[Create a Druva test user](#create-a-druva-test-user)** - toohave a counterpart of Britta Simon in Druva that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5e40d-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5e40d-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5e40d-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5e40d-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5e40d-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e40d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5e40d-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Druva alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5e40d-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Druva application.</span></span>

<span data-ttu-id="5e40d-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Druva, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5e40d-148">**tooconfigure Azure AD single sign-on with Druva, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e40d-149">Az Azure portál, a hello hello **Druva** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-149">In hello Azure portal, on hello **Druva** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="5e40d-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5e40d-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. <span data-ttu-id="5e40d-153">A hello **Druva tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-153">On hello **Druva Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    <span data-ttu-id="5e40d-155">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://cloud.druva.com/home`</span><span class="sxs-lookup"><span data-stu-id="5e40d-155">In hello **Sign-on URL** textbox, type hello URL: `https://cloud.druva.com/home`</span></span>

4. <span data-ttu-id="5e40d-156">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5e40d-156">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. <span data-ttu-id="5e40d-158">A Druva alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="5e40d-158">Your Druva application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. <span data-ttu-id="5e40d-160">A hello **felhasználói attribútumok** hello szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a lemezkép megelőző hello látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-160">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="5e40d-161">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="5e40d-161">Attribute Name</span></span>      | <span data-ttu-id="5e40d-162">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="5e40d-162">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="5e40d-163">insync\_auth\_token</span><span class="sxs-lookup"><span data-stu-id="5e40d-163">insync\_auth\_token</span></span> |<span data-ttu-id="5e40d-164">Adja meg a hello token generált érték</span><span class="sxs-lookup"><span data-stu-id="5e40d-164">Enter hello token generated value</span></span> |
    
    <span data-ttu-id="5e40d-165">a.</span><span class="sxs-lookup"><span data-stu-id="5e40d-165">a.</span></span> <span data-ttu-id="5e40d-166">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e40d-166">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="5e40d-169">b.</span><span class="sxs-lookup"><span data-stu-id="5e40d-169">b.</span></span> <span data-ttu-id="5e40d-170">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="5e40d-170">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="5e40d-171">c.</span><span class="sxs-lookup"><span data-stu-id="5e40d-171">c.</span></span> <span data-ttu-id="5e40d-172">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="5e40d-172">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="5e40d-173">hello token generált érték esetén, tekintse meg az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="5e40d-173">hello token generated value is explained later in tutorial.</span></span>
    
    <span data-ttu-id="5e40d-174">d.</span><span class="sxs-lookup"><span data-stu-id="5e40d-174">d.</span></span> <span data-ttu-id="5e40d-175">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-175">Click **Ok**.</span></span>    

7. <span data-ttu-id="5e40d-176">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-176">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5e40d-178">A hello **Druva konfigurációs** kattintson **konfigurálása Druva** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5e40d-178">On hello **Druva Configuration** section, click **Configure Druva** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5e40d-179">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5e40d-179">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. <span data-ttu-id="5e40d-181">Egy másik webes böngészőablakban jelentkezzen tooyour Druva vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5e40d-181">In a different web browser window, log in tooyour Druva company site as an administrator.</span></span>

10. <span data-ttu-id="5e40d-182">Nyissa meg túl**kezelése \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-182">Go too**Manage \> Settings**.</span></span>

    <span data-ttu-id="5e40d-183">![Beállítások](./media/active-directory-saas-druva-tutorial/ic795091.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="5e40d-183">![Settings](./media/active-directory-saas-druva-tutorial/ic795091.png "Settings")</span></span>

11. <span data-ttu-id="5e40d-184">Hello egyszeri bejelentkezési beállítások párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-184">On hello Single Sign-On Settings dialog, perform hello following steps:</span></span>

    <span data-ttu-id="5e40d-185">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-druva-tutorial/ic795092.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="5e40d-185">![Single Sign-On Settings](./media/active-directory-saas-druva-tutorial/ic795092.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="5e40d-186">a.</span><span class="sxs-lookup"><span data-stu-id="5e40d-186">a.</span></span> <span data-ttu-id="5e40d-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **azonosító szolgáltató bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5e40d-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **ID Provider Login URL** textbox.</span></span>
    
    <span data-ttu-id="5e40d-188">b.</span><span class="sxs-lookup"><span data-stu-id="5e40d-188">b.</span></span> <span data-ttu-id="5e40d-189">Beillesztés **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **azonosító szolgáltató kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5e40d-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **ID Provider Logout URL** textbox.</span></span>
    
     <span data-ttu-id="5e40d-190">c.</span><span class="sxs-lookup"><span data-stu-id="5e40d-190">c.</span></span> <span data-ttu-id="5e40d-191">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **azonosító szolgáltató tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="5e40d-191">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **ID Provider Certificate** textbox</span></span>
     
     <span data-ttu-id="5e40d-192">d.</span><span class="sxs-lookup"><span data-stu-id="5e40d-192">d.</span></span> <span data-ttu-id="5e40d-193">tooopen hello **beállítások** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-193">tooopen hello **Settings** page, click **Save**.</span></span>

12. <span data-ttu-id="5e40d-194">A hello **beállítások** kattintson **SSO jogkivonat készítése**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-194">On hello **Settings** page, click **Generate SSO Token**.</span></span>

    <span data-ttu-id="5e40d-195">![Beállítások](./media/active-directory-saas-druva-tutorial/ic795093.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="5e40d-195">![Settings](./media/active-directory-saas-druva-tutorial/ic795093.png "Settings")</span></span>

13. <span data-ttu-id="5e40d-196">A hello **egyszeri bejelentkezés hitelesítési jogkivonat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-196">On hello **Single Sign-on Authentication Token** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="5e40d-197">![Egyszeri bejelentkezési Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO jogkivonat")</span><span class="sxs-lookup"><span data-stu-id="5e40d-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO Token")</span></span>
    
    <span data-ttu-id="5e40d-198">a.</span><span class="sxs-lookup"><span data-stu-id="5e40d-198">a.</span></span> <span data-ttu-id="5e40d-199">Kattintson a **másolási**, Beillesztés érték átmásolja a hello **érték** textbox hello **attribútum hozzáadása** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5e40d-199">Click **Copy**, Paste copied value in hello **Value** textbox in hello **Add Attribute** section.</span></span>
    
    <span data-ttu-id="5e40d-200">b.</span><span class="sxs-lookup"><span data-stu-id="5e40d-200">b.</span></span> <span data-ttu-id="5e40d-201">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-201">Click **Close**.</span></span>

> [!TIP]
> <span data-ttu-id="5e40d-202">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5e40d-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5e40d-203">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5e40d-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5e40d-204">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5e40d-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5e40d-205">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5e40d-205">Create an Azure AD test user</span></span>

<span data-ttu-id="5e40d-206">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5e40d-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="5e40d-208">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5e40d-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e40d-209">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5e40d-211">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5e40d-213">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5e40d-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5e40d-215">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5e40d-217">a.</span><span class="sxs-lookup"><span data-stu-id="5e40d-217">a.</span></span> <span data-ttu-id="5e40d-218">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5e40d-219">b.</span><span class="sxs-lookup"><span data-stu-id="5e40d-219">b.</span></span> <span data-ttu-id="5e40d-220">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="5e40d-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5e40d-221">c.</span><span class="sxs-lookup"><span data-stu-id="5e40d-221">c.</span></span> <span data-ttu-id="5e40d-222">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="5e40d-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5e40d-223">d.</span><span class="sxs-lookup"><span data-stu-id="5e40d-223">d.</span></span> <span data-ttu-id="5e40d-224">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-224">Click **Create**.</span></span>
 
### <a name="create-a-druva-test-user"></a><span data-ttu-id="5e40d-225">Druva tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e40d-225">Create a Druva test user</span></span>

<span data-ttu-id="5e40d-226">A sorrend tooenable az Azure AD felhasználók toolog a tooDruva azok ki kell építenie Druva be.</span><span class="sxs-lookup"><span data-stu-id="5e40d-226">In order tooenable Azure AD users toolog in tooDruva, they must be provisioned into Druva.</span></span> <span data-ttu-id="5e40d-227">Druva hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5e40d-227">In hello case of Druva, provisioning is a manual task.</span></span>

<span data-ttu-id="5e40d-228">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5e40d-228">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e40d-229">Jelentkezzen be tooyour **Druva** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5e40d-229">Log in tooyour **Druva** company site as administrator.</span></span>

2. <span data-ttu-id="5e40d-230">Nyissa meg túl**kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-230">Go too**Manage \> Users**.</span></span>
   
   <span data-ttu-id="5e40d-231">![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795097.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="5e40d-231">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795097.png "Manage Users")</span></span>

3. <span data-ttu-id="5e40d-232">Kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-232">Click **Create New**.</span></span>
   
   <span data-ttu-id="5e40d-233">![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795098.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="5e40d-233">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795098.png "Manage Users")</span></span>

4. <span data-ttu-id="5e40d-234">Hello új felhasználó létrehozása párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5e40d-234">On hello Create New User dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="5e40d-235">![Hozzon létre Új_felhasználó](./media/active-directory-saas-druva-tutorial/ic795099.png "Új_felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="5e40d-235">![Create NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "Create NewUser")</span></span>
   
   <span data-ttu-id="5e40d-236">a.</span><span class="sxs-lookup"><span data-stu-id="5e40d-236">a.</span></span> <span data-ttu-id="5e40d-237">A hello **E-mail cím** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="5e40d-237">In hello **Email address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="5e40d-238">b.</span><span class="sxs-lookup"><span data-stu-id="5e40d-238">b.</span></span> <span data-ttu-id="5e40d-239">A hello **neve** szövegmező, írja be például a felhasználó nevét hello **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-239">In hello **Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="5e40d-240">c.</span><span class="sxs-lookup"><span data-stu-id="5e40d-240">c.</span></span> <span data-ttu-id="5e40d-241">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-241">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="5e40d-242">Bármely más Druva felhasználói fiók létrehozása eszközök vagy Druva tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="5e40d-242">You can use any other Druva user account creation tools or APIs provided by Druva tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5e40d-243">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="5e40d-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5e40d-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDruva megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5e40d-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDruva.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="5e40d-246">**tooassign Britta Simon tooDruva, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5e40d-246">**tooassign Britta Simon tooDruva, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e40d-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5e40d-249">Hello alkalmazások listában válassza ki a **Druva**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-249">In hello applications list, select **Druva**.</span></span>

    ![hello Druva hivatkozásra hello alkalmazások listája](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. <span data-ttu-id="5e40d-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5e40d-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="5e40d-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e40d-253">Click **Add** button.</span></span> <span data-ttu-id="5e40d-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e40d-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="5e40d-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5e40d-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5e40d-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e40d-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5e40d-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e40d-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5e40d-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5e40d-259">Test single sign-on</span></span>

<span data-ttu-id="5e40d-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5e40d-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5e40d-261">Ha a hozzáférési Panel hello hello Druva csempe gombra kattint, automatikusan bejelentkezett tooyour Druva alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="5e40d-261">When you click hello Druva tile in hello Access Panel, you should get automatically signed-on tooyour Druva application.</span></span>
<span data-ttu-id="5e40d-262">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5e40d-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5e40d-263">További források</span><span class="sxs-lookup"><span data-stu-id="5e40d-263">Additional resources</span></span>

* [<span data-ttu-id="5e40d-264">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5e40d-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5e40d-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5e40d-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

