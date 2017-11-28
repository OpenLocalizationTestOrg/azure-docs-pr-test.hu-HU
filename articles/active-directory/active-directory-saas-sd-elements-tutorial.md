---
title: "Oktatóanyag: Azure Active Directory-integráció SD elemekkel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SD-elemek közötti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 77949e41beb541c9fe8147b1eb2e7995e05bd753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="cad32-103">Oktatóanyag: Azure Active Directoryval integrált SD elemei</span><span class="sxs-lookup"><span data-stu-id="cad32-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="cad32-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SD olyan Azure Active Directory (Azure AD) elemet.</span><span class="sxs-lookup"><span data-stu-id="cad32-104">In this tutorial, you learn how toointegrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cad32-105">SD elemek integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-105">Integrating SD Elements with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cad32-106">Megadhatja a hozzáférés tooSD elemek rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cad32-106">You can control in Azure AD who has access tooSD Elements</span></span>
- <span data-ttu-id="cad32-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSD elemek (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cad32-107">You can enable your users tooautomatically get signed-on tooSD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cad32-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cad32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cad32-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cad32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cad32-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cad32-110">Prerequisites</span></span>

<span data-ttu-id="cad32-111">tooconfigure az Azure AD-integrációs SD elemekkel, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="cad32-111">tooconfigure Azure AD integration with SD Elements, you need hello following items:</span></span>

- <span data-ttu-id="cad32-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cad32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cad32-113">Egy SD elemek egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cad32-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cad32-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="cad32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cad32-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="cad32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cad32-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cad32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cad32-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cad32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cad32-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cad32-118">Scenario description</span></span>
<span data-ttu-id="cad32-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cad32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cad32-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cad32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cad32-121">Hello gyűjteményből SD elemek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cad32-121">Adding SD Elements from hello gallery</span></span>
2. <span data-ttu-id="cad32-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cad32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-hello-gallery"></a><span data-ttu-id="cad32-123">Hello gyűjteményből SD elemek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cad32-123">Adding SD Elements from hello gallery</span></span>
<span data-ttu-id="cad32-124">tooconfigure hello integrációs SD elemek, az Azure AD-be, meg kell tooadd SD elemek hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="cad32-124">tooconfigure hello integration of SD Elements into Azure AD, you need tooadd SD Elements from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cad32-125">**tooadd SD elemek hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cad32-125">**tooadd SD Elements from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cad32-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cad32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cad32-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cad32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cad32-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cad32-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cad32-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="cad32-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cad32-133">Hello keresési mezőbe, írja be a **SD elemek**.</span><span class="sxs-lookup"><span data-stu-id="cad32-133">In hello search box, type **SD Elements**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="cad32-135">A hello eredmények panelen válassza ki a **SD elemek**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-135">In hello results panel, select **SD Elements**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cad32-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cad32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cad32-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SD elemek.</span><span class="sxs-lookup"><span data-stu-id="cad32-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cad32-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SD elemekben tooa felhasználó az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cad32-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SD Elements is tooa user in Azure AD.</span></span> <span data-ttu-id="cad32-140">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SD elemek közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="cad32-140">In other words, a link relationship between an Azure AD user and hello related user in SD Elements needs toobe established.</span></span>

<span data-ttu-id="cad32-141">SD elemek, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="cad32-141">In SD Elements, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cad32-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése SD elemekkel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cad32-142">tooconfigure and test Azure AD single sign-on with SD Elements, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cad32-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cad32-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cad32-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cad32-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cad32-145">**[SD elemek tesztfelhasználó létrehozása](#creating-a-sd-elements-test-user)**  -toohave egy megfelelője a Britta Simon a felhasználó ábrázolása csatolt toohello az Azure AD SD elemeket.</span><span class="sxs-lookup"><span data-stu-id="cad32-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - toohave a counterpart of Britta Simon in SD Elements that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cad32-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cad32-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cad32-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="cad32-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cad32-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cad32-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cad32-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SD-elemek alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="cad32-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="cad32-150">**az Azure AD tooconfigure egyszeri bejelentkezés SD elemekkel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cad32-150">**tooconfigure Azure AD single sign-on with SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="cad32-151">Az Azure portál, a hello hello **SD elemek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cad32-151">In hello Azure portal, on hello **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cad32-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cad32-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="cad32-155">A hello **SD elemek tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-155">On hello **SD Elements Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="cad32-157">a.</span><span class="sxs-lookup"><span data-stu-id="cad32-157">a.</span></span> <span data-ttu-id="cad32-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="cad32-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="cad32-159">b.</span><span class="sxs-lookup"><span data-stu-id="cad32-159">b.</span></span> <span data-ttu-id="cad32-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="cad32-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cad32-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="cad32-161">These values are not real.</span></span> <span data-ttu-id="cad32-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="cad32-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="cad32-163">Ügyfél [SD elemek támogatási csoport](mailto:support@sdelements.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cad32-163">Contact [SD Elements support team](mailto:support@sdelements.com) tooget these values.</span></span>

4. <span data-ttu-id="cad32-164">SD elemek alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="cad32-164">SD Elements application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="cad32-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="cad32-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="cad32-166">Ezek az attribútumok értékének hello kezelheti hello **"Felhasználói attribútum"** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="cad32-166">You can manage hello values of these attributes from hello **"User Attribute"** tab of hello application.</span></span> <span data-ttu-id="cad32-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="cad32-167">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="cad32-169">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span> 

    | <span data-ttu-id="cad32-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="cad32-170">Attribute Name</span></span> | <span data-ttu-id="cad32-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="cad32-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="cad32-172">E-mailek</span><span class="sxs-lookup"><span data-stu-id="cad32-172">email</span></span> |<span data-ttu-id="cad32-173">User.mail</span><span class="sxs-lookup"><span data-stu-id="cad32-173">user.mail</span></span> |
    | <span data-ttu-id="cad32-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="cad32-174">firstname</span></span> |<span data-ttu-id="cad32-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="cad32-175">user.givenname</span></span> |
    | <span data-ttu-id="cad32-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="cad32-176">lastname</span></span> |<span data-ttu-id="cad32-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="cad32-177">user.surname</span></span> |

    <span data-ttu-id="cad32-178">a.</span><span class="sxs-lookup"><span data-stu-id="cad32-178">a.</span></span> <span data-ttu-id="cad32-179">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cad32-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="cad32-182">b.</span><span class="sxs-lookup"><span data-stu-id="cad32-182">b.</span></span> <span data-ttu-id="cad32-183">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="cad32-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="cad32-184">c.</span><span class="sxs-lookup"><span data-stu-id="cad32-184">c.</span></span> <span data-ttu-id="cad32-185">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="cad32-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="cad32-186">d.</span><span class="sxs-lookup"><span data-stu-id="cad32-186">d.</span></span> <span data-ttu-id="cad32-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="cad32-188">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cad32-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="cad32-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cad32-192">A hello **SD elemek konfigurációs** kattintson **SD-elemek konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="cad32-192">On hello **SD Elements Configuration** section, click **Configure SD Elements** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cad32-193">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="cad32-193">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="cad32-195">tooget egyszeri bejelentkezés engedélyezve van, lépjen kapcsolatba a [SD elemek támogatási csoport](mailto:support@sdelements.com) és adja meg a letöltött tanúsítvány-fájl hello.</span><span class="sxs-lookup"><span data-stu-id="cad32-195">tooget single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with hello downloaded certificate file.</span></span> 

10. <span data-ttu-id="cad32-196">Egy másik böngészőablakban, bejelentkezés tooyour SD elemek Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="cad32-196">In a different browser window, sign-on tooyour SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="cad32-197">Hello hello felső menüben kattintson a **rendszer**, majd **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cad32-197">In hello menu on hello top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="cad32-199">A hello **egyszeri bejelentkezési beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-199">On hello **Single Sign-On Settings** dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="cad32-201">a.</span><span class="sxs-lookup"><span data-stu-id="cad32-201">a.</span></span> <span data-ttu-id="cad32-202">Mint **egyszeri bejelentkezés típusa**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="cad32-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="cad32-203">b.</span><span class="sxs-lookup"><span data-stu-id="cad32-203">b.</span></span> <span data-ttu-id="cad32-204">A hello **Identity Provider Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cad32-204">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="cad32-205">c.</span><span class="sxs-lookup"><span data-stu-id="cad32-205">c.</span></span> <span data-ttu-id="cad32-206">A hello **identitás szolgáltató egyszeri bejelentkezési szolgáltatás** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cad32-206">In hello **Identity Provider Single Sign-On Service** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="cad32-207">d.</span><span class="sxs-lookup"><span data-stu-id="cad32-207">d.</span></span> <span data-ttu-id="cad32-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="cad32-209">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="cad32-209">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cad32-210">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="cad32-210">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cad32-211">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cad32-211">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cad32-212">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cad32-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="cad32-213">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="cad32-213">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cad32-215">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="cad32-215">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cad32-216">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cad32-216">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cad32-218">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cad32-218">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cad32-220">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cad32-220">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cad32-222">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-222">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cad32-224">a.</span><span class="sxs-lookup"><span data-stu-id="cad32-224">a.</span></span> <span data-ttu-id="cad32-225">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cad32-225">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cad32-226">b.</span><span class="sxs-lookup"><span data-stu-id="cad32-226">b.</span></span> <span data-ttu-id="cad32-227">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cad32-227">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cad32-228">c.</span><span class="sxs-lookup"><span data-stu-id="cad32-228">c.</span></span> <span data-ttu-id="cad32-229">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cad32-229">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cad32-230">d.</span><span class="sxs-lookup"><span data-stu-id="cad32-230">d.</span></span> <span data-ttu-id="cad32-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="cad32-232">SD elemek tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cad32-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="cad32-233">hello ebben a szakaszban célja toocreate SD elemek Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cad32-233">hello objective of this section is toocreate a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="cad32-234">Az SD-elemek hello esetben kézi tevékenység SD elemek felhasználók létrehozásáról.</span><span class="sxs-lookup"><span data-stu-id="cad32-234">In hello case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="cad32-235">**toocreate Britta Simon SD elemekben, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cad32-235">**toocreate Britta Simon in SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="cad32-236">A webböngésző ablakának, bejelentkezés tooyour SD elemek vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cad32-236">In a web browser window, sign-on tooyour SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="cad32-237">Hello hello felső menüben kattintson a **felhasználókezelés**, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="cad32-237">In hello menu on hello top, click **User Management**, and then **Users**.</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="cad32-239">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cad32-239">Click **Add New User**.</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="cad32-241">A hello **új felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cad32-241">On hello **Add New User** dialog, perform hello following steps:</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="cad32-243">a.</span><span class="sxs-lookup"><span data-stu-id="cad32-243">a.</span></span> <span data-ttu-id="cad32-244">A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cad32-244">In hello **E-mail** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="cad32-245">b.</span><span class="sxs-lookup"><span data-stu-id="cad32-245">b.</span></span> <span data-ttu-id="cad32-246">A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cad32-246">In hello **First Name** textbox, enter hello first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="cad32-247">c.</span><span class="sxs-lookup"><span data-stu-id="cad32-247">c.</span></span> <span data-ttu-id="cad32-248">A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cad32-248">In hello **Last Name** textbox, enter hello last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="cad32-249">d.</span><span class="sxs-lookup"><span data-stu-id="cad32-249">d.</span></span> <span data-ttu-id="cad32-250">Mint **szerepkör**, jelölje be **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="cad32-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="cad32-251">e.</span><span class="sxs-lookup"><span data-stu-id="cad32-251">e.</span></span> <span data-ttu-id="cad32-252">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cad32-252">Click **Create User**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cad32-253">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cad32-253">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cad32-254">Ebben a szakaszban engedélyezése Azure egyszeri bejelentkezés Britta Simon toouse tooSD elemek hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="cad32-254">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSD Elements.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cad32-256">**tooassign Britta Simon tooSD elemek, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cad32-256">**tooassign Britta Simon tooSD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="cad32-257">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cad32-257">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cad32-259">Hello alkalmazások listában válassza ki a **SD elemek**.</span><span class="sxs-lookup"><span data-stu-id="cad32-259">In hello applications list, select **SD Elements**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="cad32-261">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cad32-261">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cad32-263">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cad32-263">Click **Add** button.</span></span> <span data-ttu-id="cad32-264">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cad32-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cad32-266">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cad32-266">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cad32-267">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cad32-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cad32-268">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cad32-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cad32-269">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cad32-269">Testing single sign-on</span></span>

<span data-ttu-id="cad32-270">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="cad32-270">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
  
<span data-ttu-id="cad32-271">Hello SD elemek hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour SD elemek alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cad32-271">When you click hello SD Elements tile in hello Access Panel, you should get automatically signed-on tooyour SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cad32-272">További források</span><span class="sxs-lookup"><span data-stu-id="cad32-272">Additional resources</span></span>

* [<span data-ttu-id="cad32-273">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="cad32-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cad32-274">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cad32-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

