---
title: "Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Amazon Web Services (AWS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="9bc89-103">Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="9bc89-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="9bc89-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Amazon Web Services (AWS) az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9bc89-104">In this tutorial, you learn how toointegrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9bc89-105">Amazon Web Services (AWS) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9bc89-106">Megadhatja a hozzáférés tooAmazon Web Services (AWS) rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9bc89-106">You can control in Azure AD who has access tooAmazon Web Services (AWS)</span></span>
- <span data-ttu-id="9bc89-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAmazon Web Services (AWS) (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9bc89-107">You can enable your users tooautomatically get signed-on tooAmazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9bc89-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9bc89-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9bc89-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9bc89-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="9bc89-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9bc89-110">Prerequisites</span></span>

<span data-ttu-id="9bc89-111">tooconfigure az Azure AD-integrációs Amazon Web Services (AWS), a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="9bc89-111">tooconfigure Azure AD integration with Amazon Web Services (AWS), you need hello following items:</span></span>

- <span data-ttu-id="9bc89-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9bc89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9bc89-113">Amazon Web Services (AWS) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9bc89-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9bc89-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9bc89-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9bc89-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9bc89-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9bc89-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9bc89-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9bc89-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9bc89-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9bc89-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9bc89-118">Scenario description</span></span>
<span data-ttu-id="9bc89-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9bc89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9bc89-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9bc89-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9bc89-121">Amazon Web Services (AWS) hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9bc89-121">Adding Amazon Web Services (AWS) from hello gallery</span></span>
2. <span data-ttu-id="9bc89-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9bc89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a><span data-ttu-id="9bc89-123">Amazon Web Services (AWS) hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9bc89-123">Adding Amazon Web Services (AWS) from hello gallery</span></span>
<span data-ttu-id="9bc89-124">tooconfigure hello integrációs Amazon Web Services (AWS) az Azure AD-be, meg kell tooadd Amazon Web Services (AWS) hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9bc89-124">tooconfigure hello integration of Amazon Web Services (AWS) into Azure AD, you need tooadd Amazon Web Services (AWS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9bc89-125">**tooadd Amazon Web Services (AWS) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9bc89-125">**tooadd Amazon Web Services (AWS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bc89-126">A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9bc89-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9bc89-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9bc89-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9bc89-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9bc89-133">Hello keresési mezőbe, írja be a **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-133">In hello search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="9bc89-135">A hello eredmények panelen válassza ki a **Amazon Web Services (AWS)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-135">In hello results panel, select **Amazon Web Services (AWS)**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9bc89-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9bc89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9bc89-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az Amazon Web Services (AWS) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="9bc89-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9bc89-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Amazon Web Services (AWS) tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9bc89-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Amazon Web Services (AWS) is tooa user in Azure AD.</span></span> <span data-ttu-id="9bc89-140">Ez azt jelenti hello kapcsolódó felhasználói Amazon Web Services (AWS) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9bc89-140">In other words, a link relationship between an Azure AD user and hello related user in Amazon Web Services (AWS) needs toobe established.</span></span>

<span data-ttu-id="9bc89-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9bc89-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="9bc89-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése az Amazon Web Services (AWS), a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9bc89-142">tooconfigure and test Azure AD single sign-on with Amazon Web Services (AWS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9bc89-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9bc89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9bc89-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9bc89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9bc89-145">**[Az Amazon Web Services tesztfelhasználó létrehozása](#creating-an-amazon-web-services-test-user)**  -toohave Britta Simon az Amazon Web Services (AWS), amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="9bc89-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - toohave a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9bc89-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9bc89-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9bc89-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9bc89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9bc89-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bc89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9bc89-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Amazon Web Services (AWS) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9bc89-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="9bc89-150">**az Amazon Web Services (AWS), az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9bc89-150">**tooconfigure Azure AD single sign-on with Amazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9bc89-151">Az Azure-portálon hello a hello **Amazon Web Services (AWS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-151">In hello Azure Portal, on hello **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9bc89-153">A hello **egyszeri bejelentkezés** párbeszédpanelen, **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9bc89-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="9bc89-155">A hello **Amazon Web Services (AWS) tartományhoz és URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="9bc89-155">On hello **Amazon Web Services (AWS) Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="9bc89-157">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9bc89-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="9bc89-159">hello Amazon Web Services (AWS) alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="9bc89-159">hello Amazon Web Services (AWS) Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="9bc89-160">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9bc89-160">Please configure hello following claims for this application.</span></span> <span data-ttu-id="9bc89-161">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="9bc89-161">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="9bc89-162">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="9bc89-162">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="9bc89-164">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-164">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="9bc89-165">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="9bc89-165">Attribute Name</span></span>  | <span data-ttu-id="9bc89-166">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="9bc89-166">Attribute Value</span></span> | <span data-ttu-id="9bc89-167">Namespace</span><span class="sxs-lookup"><span data-stu-id="9bc89-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="9bc89-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="9bc89-168">RoleSessionName</span></span> | <span data-ttu-id="9bc89-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9bc89-169">user.userprincipalname</span></span> | <span data-ttu-id="9bc89-170">https://AWS.amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="9bc89-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="9bc89-171">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="9bc89-171">Role</span></span>            | <span data-ttu-id="9bc89-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="9bc89-172">user.assignedroles</span></span> |  <span data-ttu-id="9bc89-173">https://AWS.amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="9bc89-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="9bc89-174">Az AWS konzol minden hello szerepkörök az Azure AD toofetch kiépítési tooconfigure hello felhasználó van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9bc89-174">You need tooconfigure hello user provisioning in Azure AD toofetch all hello roles from AWS Console.</span></span> <span data-ttu-id="9bc89-175">Tekintse meg az alábbi lépéseket kiépítés hello.</span><span class="sxs-lookup"><span data-stu-id="9bc89-175">Please refer hello provisioning steps below.</span></span>

    <span data-ttu-id="9bc89-176">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-176">a.</span></span> <span data-ttu-id="9bc89-177">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9bc89-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="9bc89-179">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-179">b.</span></span> <span data-ttu-id="9bc89-180">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="9bc89-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9bc89-182">c.</span><span class="sxs-lookup"><span data-stu-id="9bc89-182">c.</span></span> <span data-ttu-id="9bc89-183">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="9bc89-183">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="9bc89-184">Hello Namespace érték hozzáadásával a fent megadott.</span><span class="sxs-lookup"><span data-stu-id="9bc89-184">Add hello Namespace value as given above.</span></span>
    
    <span data-ttu-id="9bc89-185">d.</span><span class="sxs-lookup"><span data-stu-id="9bc89-185">d.</span></span> <span data-ttu-id="9bc89-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-186">Click **Ok**.</span></span>

7. <span data-ttu-id="9bc89-187">Kattintson a **mentése** Azure toosave hello beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-187">Click **Save** button toosave hello settings on Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9bc89-189">Egy másik böngészőablakban, bejelentkezés tooyour Amazon Web Services (AWS) vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9bc89-189">In a different browser window, sign-on tooyour Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="9bc89-190">Kattintson a **konzol otthoni**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-190">Click **Console Home**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][11]

10. <span data-ttu-id="9bc89-192">Kattintson a **IAM** a **biztonsági, identitás- & megfelelőségi** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9bc89-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][12]

11. <span data-ttu-id="9bc89-194">Kattintson a **identitás-szolgáltatóktól**, és kattintson a **létrehozása szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][13]

12. <span data-ttu-id="9bc89-196">A hello **szolgáltató konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-196">On hello **Configure Provider** dialog page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][14]
 
    <span data-ttu-id="9bc89-198">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-198">a.</span></span> <span data-ttu-id="9bc89-199">Mint **szolgáltatótípust**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="9bc89-200">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-200">b.</span></span> <span data-ttu-id="9bc89-201">A hello **szolgáltatónevet** szövegmező, írja be a szolgáltató nevét (pl.: *fahulladékok*).</span><span class="sxs-lookup"><span data-stu-id="9bc89-201">In hello **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="9bc89-202">c.</span><span class="sxs-lookup"><span data-stu-id="9bc89-202">c.</span></span> <span data-ttu-id="9bc89-203">tooupload a letöltött metaadatfájl, kattintson a **Choose File**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-203">tooupload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="9bc89-204">d.</span><span class="sxs-lookup"><span data-stu-id="9bc89-204">d.</span></span> <span data-ttu-id="9bc89-205">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="9bc89-206">A hello **ellenőrizze a szolgáltató adatait** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-206">On hello **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][15]

14. <span data-ttu-id="9bc89-208">Kattintson a **szerepkörök**, és kattintson a **hozzon létre új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][16]

15. <span data-ttu-id="9bc89-210">A hello **szerepkörnév beállítása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-210">On hello **Set Role Name** dialog, perform hello following steps:</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][17] 

    <span data-ttu-id="9bc89-212">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-212">a.</span></span> <span data-ttu-id="9bc89-213">A hello **szerepkörnév** szövegmező, írja be a szerepkör nevét (pl.: *tesztfelhasználó néven*).</span><span class="sxs-lookup"><span data-stu-id="9bc89-213">In hello **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="9bc89-214">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-214">b.</span></span> <span data-ttu-id="9bc89-215">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="9bc89-216">A hello **szerepkör típusának kiválasztása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-216">On hello **Select Role Type** dialog, perform hello following steps:</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][18] 

    <span data-ttu-id="9bc89-218">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-218">a.</span></span> <span data-ttu-id="9bc89-219">Válassza ki **Identity Provider hozzáférési szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="9bc89-220">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-220">b.</span></span> <span data-ttu-id="9bc89-221">A hello **Grant webes egyszeri bejelentkezés (WebSSO) hozzáférési tooSAML szolgáltatók** területén kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-221">In hello **Grant Web Single Sign-On (WebSSO) access tooSAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="9bc89-222">A hello **létre megbízható** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-222">On hello **Establish Trust** dialog, perform hello following steps:</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása][19] 

    <span data-ttu-id="9bc89-224">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-224">a.</span></span> <span data-ttu-id="9bc89-225">SAML-szolgáltatóként, válassza ki a korábban létrehozott hello SAML-szolgáltatót (pl.: *fahulladékok*)</span><span class="sxs-lookup"><span data-stu-id="9bc89-225">As SAML provider, select hello SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="9bc89-226">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-226">b.</span></span> <span data-ttu-id="9bc89-227">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="9bc89-228">A hello **szerepkör megbízható ellenőrizze** párbeszédpanel, kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-228">On hello **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][32]

19. <span data-ttu-id="9bc89-230">A hello **csatolása házirend** párbeszédpanel, kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-230">On hello **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][33]

20. <span data-ttu-id="9bc89-232">A hello **felülvizsgálati** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-232">On hello **Review** dialog, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][34]
 
    <span data-ttu-id="9bc89-234">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-234">a.</span></span> <span data-ttu-id="9bc89-235">Kattintson a **szerepkör létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-235">Click **Create Role**.</span></span>

    <span data-ttu-id="9bc89-236">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-236">b.</span></span> <span data-ttu-id="9bc89-237">Igény szerint annyi szerepköröket hozhat létre, és oldalváltozókhoz toohello identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="9bc89-237">Create as many roles as needed and map them toohello Identity Provider.</span></span>

21. <span data-ttu-id="9bc89-238">Mostantól beállíthatja hello felhasználók toofetch átadásához AWS összes hello szerepkörei</span><span class="sxs-lookup"><span data-stu-id="9bc89-238">Now configure hello user provisioning toofetch all hello roles from AWS</span></span>

    <span data-ttu-id="9bc89-239">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-239">a.</span></span> <span data-ttu-id="9bc89-240">A hello AWS konzol jelentkezzen be a rendszergazdafiók</span><span class="sxs-lookup"><span data-stu-id="9bc89-240">In hello AWS Console login with your root account</span></span>

    <span data-ttu-id="9bc89-241">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-241">b.</span></span> <span data-ttu-id="9bc89-242">A hello jobb felső sarokban kattintson a nevére, és kattintson a hello **saját biztonsági hitelesítő adatok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9bc89-242">In hello top right corner click your name and then click hello **My Security Credentials** option.</span></span> <span data-ttu-id="9bc89-243">Figyelmeztető üzenet, ekkor megnyílik egy olyan képernyőt.</span><span class="sxs-lookup"><span data-stu-id="9bc89-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="9bc89-244">Hello gombra **biztonsági hitelesítő adatok** gomb toopass hello képernyő.</span><span class="sxs-lookup"><span data-stu-id="9bc89-244">Click hello button **Security Credentials** button toopass hello screen.</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][36]

       ![Egyszeri bejelentkezés konfigurálása][37]

    <span data-ttu-id="9bc89-247">c.</span><span class="sxs-lookup"><span data-stu-id="9bc89-247">c.</span></span> <span data-ttu-id="9bc89-248">A Tárelérési kulcsok hello szakaszban kattintson hello **új kulcs létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-248">In hello Access Keys section click hello **Create New Access Key** button.</span></span> <span data-ttu-id="9bc89-249">Ezt követően hello hozzáférési kulcs Azonosítóját és a token értékét.</span><span class="sxs-lookup"><span data-stu-id="9bc89-249">This generates hello Access Key ID and a token value.</span></span>
    
       ![Egyszeri bejelentkezés konfigurálása][38]

    <span data-ttu-id="9bc89-251">d.</span><span class="sxs-lookup"><span data-stu-id="9bc89-251">d.</span></span> <span data-ttu-id="9bc89-252">Másolja a következő két értéket, és töltse le a is maga után, hogy ne vesszenek el.</span><span class="sxs-lookup"><span data-stu-id="9bc89-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="9bc89-253">e.</span><span class="sxs-lookup"><span data-stu-id="9bc89-253">e.</span></span> <span data-ttu-id="9bc89-254">Az Azure-portálon hello hello Amazon Web Services (AWS) alkalmazás integráció lapján, kattintson a **kiépítési**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-254">In hello Azure Portal, on hello Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][35]

    <span data-ttu-id="9bc89-256">f.</span><span class="sxs-lookup"><span data-stu-id="9bc89-256">f.</span></span> <span data-ttu-id="9bc89-257">Hello Kiépítési mód beállítása túl**automatikus**</span><span class="sxs-lookup"><span data-stu-id="9bc89-257">Set hello Provisioning mode too**Automatic**</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][39]

    <span data-ttu-id="9bc89-259">g.</span><span class="sxs-lookup"><span data-stu-id="9bc89-259">g.</span></span> <span data-ttu-id="9bc89-260">Most már a hello **clientsecret** és **titkos Token** illessze be a hello tartozó értékek, amelyek az AWS konzol másolta.</span><span class="sxs-lookup"><span data-stu-id="9bc89-260">Now in hello **clientsecret** and **Secret Token** paste hello corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="9bc89-261">h.</span><span class="sxs-lookup"><span data-stu-id="9bc89-261">h.</span></span> <span data-ttu-id="9bc89-262">Kattinthat a hello **kapcsolat tesztelése** tootest hello kapcsolat gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-262">You can click hello **Test Connection** button tootest hello connectivity.</span></span> <span data-ttu-id="9bc89-263">Miután művelet sikeres majd elindíthatja összekötő kiépítés hello.</span><span class="sxs-lookup"><span data-stu-id="9bc89-263">Once that is successful then you can start hello provisioning connector.</span></span>
       
       ![Egyszeri bejelentkezés konfigurálása][40]

    <span data-ttu-id="9bc89-265">i.</span><span class="sxs-lookup"><span data-stu-id="9bc89-265">i.</span></span> <span data-ttu-id="9bc89-266">Most már engedélyezheti a kiépítési állapot hello túl**a**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-266">Now enable hello Provisioning Status too**On**.</span></span> <span data-ttu-id="9bc89-267">Hello szerepkörök beolvasása a hello alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="9bc89-267">This starts fetching hello roles from hello application.</span></span>

       ![Egyszeri bejelentkezés konfigurálása][41]

    > [!NOTE]
    > <span data-ttu-id="9bc89-269">Az Azure AD-kiépítés szolgáltatásának futtatásakor minden egyes idő toosync hello szerepkörök az AWS után.</span><span class="sxs-lookup"><span data-stu-id="9bc89-269">Azure AD Provisioning service runs every after some time toosync hello roles from AWS.</span></span> <span data-ttu-id="9bc89-270">Megtekintheti az összes hello identitásszolgáltató csatolt AWS szerepkörök az Azure AD-be és hello alkalmazás toousers vagy csoportok hozzárendelésekor is használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="9bc89-270">You should see all hello Identity Provider attached AWS roles into Azure AD and you can use them while assigning hello application toousers or groups.</span></span>

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9bc89-271">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bc89-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="9bc89-272">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9bc89-272">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9bc89-274">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9bc89-274">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bc89-275">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-275">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9bc89-277">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="9bc89-277">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9bc89-279">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9bc89-279">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9bc89-281">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-281">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9bc89-283">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-283">a.</span></span> <span data-ttu-id="9bc89-284">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-284">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9bc89-285">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-285">b.</span></span> <span data-ttu-id="9bc89-286">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9bc89-286">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9bc89-287">c.</span><span class="sxs-lookup"><span data-stu-id="9bc89-287">c.</span></span> <span data-ttu-id="9bc89-288">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-288">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9bc89-289">d.</span><span class="sxs-lookup"><span data-stu-id="9bc89-289">d.</span></span> <span data-ttu-id="9bc89-290">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="9bc89-291">Az Amazon Web Services tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bc89-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="9bc89-292">A sorrend tooenable az Azure AD felhasználók toolog a Web Services (AWS) tooAmazon azok ki kell építenie az Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9bc89-292">In order tooenable Azure AD users toolog in tooAmazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="9bc89-293">Hello esetben Amazon Web Services (AWS) kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9bc89-293">In hello case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="9bc89-294">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9bc89-294">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bc89-295">Jelentkezzen be tooyour **Amazon Web Services (AWS)** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9bc89-295">Log in tooyour **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="9bc89-296">Kattintson a hello **konzol otthoni** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-296">Click hello **Console Home** icon.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][11]

3. <span data-ttu-id="9bc89-298">Kattintson az identitás és hozzáférés-kezelést.</span><span class="sxs-lookup"><span data-stu-id="9bc89-298">Click Identity and Access Management.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][28]

4. <span data-ttu-id="9bc89-300">Hello irányítópultot, kattintson **felhasználók**, és kattintson a **hozzon létre új felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-300">In hello Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][29]

5. <span data-ttu-id="9bc89-302">Hello felhasználó létrehozása párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="9bc89-302">On hello Create User dialog, perform hello following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][30]   
    
    <span data-ttu-id="9bc89-304">a.</span><span class="sxs-lookup"><span data-stu-id="9bc89-304">a.</span></span> <span data-ttu-id="9bc89-305">A hello **adja meg a felhasználói neveket** szövegmezőből, írja be a Brita Simon felhasználónevet (userprincipalname), az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9bc89-305">In hello **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="9bc89-306">b.</span><span class="sxs-lookup"><span data-stu-id="9bc89-306">b.</span></span> <span data-ttu-id="9bc89-307">Kattintson a **létrehozása.**</span><span class="sxs-lookup"><span data-stu-id="9bc89-307">Click **Create.**</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9bc89-308">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9bc89-308">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9bc89-309">Ebben a szakaszban engedélyezése Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooAmazon Web Services (AWS) megadása.</span><span class="sxs-lookup"><span data-stu-id="9bc89-309">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAmazon Web Services (AWS).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9bc89-311">**tooassign Britta Simon tooAmazon Web Services (AWS), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9bc89-311">**tooassign Britta Simon tooAmazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9bc89-312">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-312">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9bc89-314">Hello alkalmazások listában válassza ki a **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-314">In hello applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="9bc89-316">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9bc89-316">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9bc89-318">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9bc89-318">Click **Add** button.</span></span> <span data-ttu-id="9bc89-319">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9bc89-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9bc89-321">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9bc89-321">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9bc89-322">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9bc89-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9bc89-323">A **Szerepkörválasztás** lapon adja meg, jelölje be hello megfelelő hello felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="9bc89-323">On **Select Role** tab, select hello appropriate role for hello user.</span></span> <span data-ttu-id="9bc89-324">Ezek a szerepkörök jelennek meg hello szerepkör nevét és az identitás-szolgáltató neve.</span><span class="sxs-lookup"><span data-stu-id="9bc89-324">All these roles are shown with hello role name and identity provider name.</span></span> <span data-ttu-id="9bc89-325">Így egyszerűbb azonosítani AWS hello szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="9bc89-325">This way you can easily identify hello roles from AWS.</span></span>

8. <span data-ttu-id="9bc89-326">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9bc89-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9bc89-327">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9bc89-327">Testing single sign-on</span></span>

<span data-ttu-id="9bc89-328">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9bc89-328">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9bc89-329">Hello hello hozzáférési Panel Amazon Web Services (AWS) csempére kattintva kapja meg automatikusan bejelentkezett tooyour Amazon Web Services (AWS) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9bc89-329">When you click hello Amazon Web Services (AWS) tile in hello Access Panel, you should get automatically signed-on tooyour Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9bc89-330">További források</span><span class="sxs-lookup"><span data-stu-id="9bc89-330">Additional resources</span></span>

* [<span data-ttu-id="9bc89-331">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9bc89-331">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9bc89-332">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9bc89-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
