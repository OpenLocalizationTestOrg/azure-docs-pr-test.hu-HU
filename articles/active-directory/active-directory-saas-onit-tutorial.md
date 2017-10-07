---
title: "Oktatóanyag: Azure Active Directoryval integrált Onit |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Onit között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 9e12449e5bf7f169b3cadfaa12438ac5d52ed8f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="c9b87-103">Oktatóanyag: Azure Active Directoryval integrált Onit</span><span class="sxs-lookup"><span data-stu-id="c9b87-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="c9b87-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Onit az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c9b87-104">In this tutorial, you learn how toointegrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9b87-105">Onit integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-105">Integrating Onit with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c9b87-106">Az Azure AD hozzáférési tooOnit rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="c9b87-106">You can control in Azure AD who has access tooOnit.</span></span>
- <span data-ttu-id="c9b87-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOnit (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="c9b87-107">You can enable your users tooautomatically get signed-on tooOnit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c9b87-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="c9b87-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="c9b87-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9b87-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9b87-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9b87-110">Prerequisites</span></span>

<span data-ttu-id="c9b87-111">az Azure AD integrálása Onit tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-111">tooconfigure Azure AD integration with Onit, you need hello following items:</span></span>

- <span data-ttu-id="c9b87-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c9b87-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9b87-113">Egy Onit egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c9b87-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9b87-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c9b87-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9b87-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c9b87-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9b87-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9b87-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9b87-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9b87-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9b87-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c9b87-118">Scenario description</span></span>

<span data-ttu-id="c9b87-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c9b87-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9b87-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c9b87-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9b87-121">Hello gyűjteményből Onit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9b87-121">Adding Onit from hello gallery</span></span>
2. <span data-ttu-id="c9b87-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c9b87-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-hello-gallery"></a><span data-ttu-id="c9b87-123">Hello gyűjteményből Onit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9b87-123">Adding Onit from hello gallery</span></span>
<span data-ttu-id="c9b87-124">tooconfigure hello integrációja Onit az Azure AD-be, meg kell tooadd Onit hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c9b87-124">tooconfigure hello integration of Onit into Azure AD, you need tooadd Onit from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c9b87-125">**tooadd Onit hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c9b87-125">**tooadd Onit from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9b87-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="c9b87-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c9b87-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="c9b87-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c9b87-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="c9b87-133">Hello keresési mezőbe, írja be a **Onit**, jelölje be **Onit** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-133">In hello search box, type **Onit**, select **Onit** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Onit](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c9b87-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9b87-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c9b87-136">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Onit</span><span class="sxs-lookup"><span data-stu-id="c9b87-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c9b87-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Onit tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c9b87-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Onit is tooa user in Azure AD.</span></span> <span data-ttu-id="c9b87-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Onit közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c9b87-138">In other words, a link relationship between an Azure AD user and hello related user in Onit needs toobe established.</span></span>

<span data-ttu-id="c9b87-139">Onit, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c9b87-139">In Onit, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c9b87-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Onit-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c9b87-140">tooconfigure and test Azure AD single sign-on with Onit, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c9b87-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c9b87-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c9b87-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9b87-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9b87-143">**[Hozzon létre egy Onit tesztfelhasználó](#create-an-onit-test-user)**  -toohave egy megfelelője a Britta Simon a Onit, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c9b87-143">**[Create an Onit test user](#create-an-onit-test-user)** - toohave a counterpart of Britta Simon in Onit that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9b87-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c9b87-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9b87-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c9b87-145">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c9b87-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9b87-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c9b87-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Onit alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c9b87-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="c9b87-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Onit, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c9b87-148">**tooconfigure Azure AD single sign-on with Onit, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9b87-149">Az Azure portál, a hello hello **Onit** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-149">In hello Azure portal, on hello **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="c9b87-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c9b87-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="c9b87-153">A hello **Onit tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-153">On hello **Onit Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Onit tartomány és az URL-címek](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="c9b87-155">a.</span><span class="sxs-lookup"><span data-stu-id="c9b87-155">a.</span></span> <span data-ttu-id="c9b87-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="c9b87-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="c9b87-157">b.</span><span class="sxs-lookup"><span data-stu-id="c9b87-157">b.</span></span> <span data-ttu-id="c9b87-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="c9b87-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9b87-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c9b87-159">These values are not real.</span></span> <span data-ttu-id="c9b87-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="c9b87-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c9b87-161">Ügyfél [Onit ügyfél-támogatási csoport](https://www.onit.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c9b87-161">Contact [Onit Client support team](https://www.onit.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="c9b87-162">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="c9b87-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="c9b87-164">Onit alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="c9b87-164">Onit application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c9b87-165">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c9b87-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="c9b87-166">Ezek az attribútumok értékének hello kezelheti hello **"Atrribute"** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="c9b87-166">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="c9b87-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="c9b87-167">hello following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="c9b87-169">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="c9b87-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c9b87-170">Attribute Name</span></span> | <span data-ttu-id="c9b87-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c9b87-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="c9b87-172">E-mailek</span><span class="sxs-lookup"><span data-stu-id="c9b87-172">email</span></span> | <span data-ttu-id="c9b87-173">User.mail</span><span class="sxs-lookup"><span data-stu-id="c9b87-173">user.mail</span></span> |
    
    <span data-ttu-id="c9b87-174">a.</span><span class="sxs-lookup"><span data-stu-id="c9b87-174">a.</span></span> <span data-ttu-id="c9b87-175">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9b87-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c9b87-178">b.</span><span class="sxs-lookup"><span data-stu-id="c9b87-178">b.</span></span> <span data-ttu-id="c9b87-179">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="c9b87-179">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="c9b87-180">c.</span><span class="sxs-lookup"><span data-stu-id="c9b87-180">c.</span></span> <span data-ttu-id="c9b87-181">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="c9b87-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="c9b87-182">d.</span><span class="sxs-lookup"><span data-stu-id="c9b87-182">d.</span></span> <span data-ttu-id="c9b87-183">Hagyja hello **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="c9b87-183">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="c9b87-184">e.</span><span class="sxs-lookup"><span data-stu-id="c9b87-184">e.</span></span> <span data-ttu-id="c9b87-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-185">Click **Ok**.</span></span>

7. <span data-ttu-id="c9b87-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c9b87-188">A hello **Onit konfigurációs** kattintson **konfigurálása Onit** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c9b87-188">On hello **Onit Configuration** section, click **Configure Onit** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c9b87-189">Másolás hello **Sign-Out URL, SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c9b87-189">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Onit konfiguráció](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="c9b87-191">Egy másik webes böngészőablakban jelentkezzen be a Onit vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9b87-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="c9b87-192">Hello hello felső menüben kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-192">In hello menu on hello top, click **Administration**.</span></span>
   
   <span data-ttu-id="c9b87-193">![Felügyeleti](./media/active-directory-saas-onit-tutorial/IC791174.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="c9b87-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="c9b87-194">Kattintson a **Szerkesztés Corporation**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="c9b87-195">![Szerkesztés Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Szerkesztés Corporation")</span><span class="sxs-lookup"><span data-stu-id="c9b87-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="c9b87-196">Kattintson a hello **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="c9b87-196">Click hello **Security** tab.</span></span>
    
    <span data-ttu-id="c9b87-197">![Vállalat adatainak szerkesztése](./media/active-directory-saas-onit-tutorial/IC791176.png "vállalat adatainak szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="c9b87-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="c9b87-198">A hello **biztonsági** lapra, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-198">On hello **Security** tab, perform hello following steps:</span></span>

    <span data-ttu-id="c9b87-199">![Egyszeri bejelentkezés](./media/active-directory-saas-onit-tutorial/IC791177.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="c9b87-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="c9b87-200">a.</span><span class="sxs-lookup"><span data-stu-id="c9b87-200">a.</span></span> <span data-ttu-id="c9b87-201">Mint **hitelesítési stratégia**, jelölje be **egyszeri bejelentkezés és a jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="c9b87-202">b.</span><span class="sxs-lookup"><span data-stu-id="c9b87-202">b.</span></span> <span data-ttu-id="c9b87-203">A **Idp célként megadott URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c9b87-203">In **Idp Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c9b87-204">c.</span><span class="sxs-lookup"><span data-stu-id="c9b87-204">c.</span></span> <span data-ttu-id="c9b87-205">A **Idp kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c9b87-205">In **Idp logout URL** textbox, paste hello value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c9b87-206">d.</span><span class="sxs-lookup"><span data-stu-id="c9b87-206">d.</span></span> <span data-ttu-id="c9b87-207">A **Idp tanúsítvány-ujjlenyomat (SHA1)** szövegmezőhöz Beillesztés hello **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="c9b87-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste hello  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="c9b87-208">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c9b87-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c9b87-209">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c9b87-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c9b87-210">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9b87-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c9b87-211">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c9b87-211">Create an Azure AD test user</span></span>

<span data-ttu-id="c9b87-212">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c9b87-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="c9b87-214">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c9b87-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9b87-215">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-215">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c9b87-217">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-217">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c9b87-219">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c9b87-219">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c9b87-221">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-221">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c9b87-223">a.</span><span class="sxs-lookup"><span data-stu-id="c9b87-223">a.</span></span> <span data-ttu-id="c9b87-224">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-224">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9b87-225">b.</span><span class="sxs-lookup"><span data-stu-id="c9b87-225">b.</span></span> <span data-ttu-id="c9b87-226">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="c9b87-226">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="c9b87-227">c.</span><span class="sxs-lookup"><span data-stu-id="c9b87-227">c.</span></span> <span data-ttu-id="c9b87-228">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c9b87-228">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="c9b87-229">d.</span><span class="sxs-lookup"><span data-stu-id="c9b87-229">d.</span></span> <span data-ttu-id="c9b87-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="c9b87-231">Hozzon létre egy Onit tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="c9b87-231">Create an Onit test user</span></span>

<span data-ttu-id="c9b87-232">A sorrend tooenable az Azure AD felhasználók toolog Onit be azok ki kell építenie Onit be.</span><span class="sxs-lookup"><span data-stu-id="c9b87-232">In order tooenable Azure AD users toolog into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="c9b87-233">Onit hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c9b87-233">In hello case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="c9b87-234">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c9b87-234">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9b87-235">Jelentkezzen be tooyour **Onit** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9b87-235">Sign on tooyour **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="c9b87-236">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="c9b87-237">![Felügyeleti](./media/active-directory-saas-onit-tutorial/IC791180.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="c9b87-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="c9b87-238">A hello **felhasználó hozzáadása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c9b87-238">On hello **Add User** dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="c9b87-239">![Felhasználó hozzáadása](./media/active-directory-saas-onit-tutorial/IC791181.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="c9b87-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="c9b87-240">Típus hello **neve** és hello **E-mail címet** egy érvényes Azure AD-fiókot a kívánt tooprovision hello kapcsolódó szövegmezők.</span><span class="sxs-lookup"><span data-stu-id="c9b87-240">Type hello **Name** and hello **Email Address** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>
  2. <span data-ttu-id="c9b87-241">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="c9b87-242">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="c9b87-242">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c9b87-243">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="c9b87-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c9b87-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOnit megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c9b87-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOnit.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="c9b87-246">**tooassign Britta Simon tooOnit, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c9b87-246">**tooassign Britta Simon tooOnit, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9b87-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c9b87-249">Hello alkalmazások listában válassza ki a **Onit**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-249">In hello applications list, select **Onit**.</span></span>

    ![hello Onit hivatkozásra hello alkalmazások listája](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="c9b87-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c9b87-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="c9b87-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b87-253">Click **Add** button.</span></span> <span data-ttu-id="c9b87-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9b87-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="c9b87-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c9b87-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c9b87-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9b87-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9b87-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9b87-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c9b87-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c9b87-259">Test single sign-on</span></span>

<span data-ttu-id="c9b87-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c9b87-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c9b87-261">Ha a hozzáférési Panel hello hello Onit csempe gombra kattint, automatikusan bejelentkezett tooyour Onit alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c9b87-261">When you click hello Onit tile in hello Access Panel, you should get automatically signed-on tooyour Onit application.</span></span>
<span data-ttu-id="c9b87-262">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c9b87-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c9b87-263">További források</span><span class="sxs-lookup"><span data-stu-id="c9b87-263">Additional resources</span></span>

* [<span data-ttu-id="c9b87-264">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c9b87-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9b87-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c9b87-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

