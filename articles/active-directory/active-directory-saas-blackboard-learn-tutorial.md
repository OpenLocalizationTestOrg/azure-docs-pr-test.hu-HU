---
title: "Oktatóanyag: Azure Active Directoryval integrált Antik további |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a további Antik között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: e94cdd6eaf876d4f66bdd783c442dc468f104e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="c3786-103">Oktatóanyag: Azure Active Directoryval integrált Antik tudnivalók</span><span class="sxs-lookup"><span data-stu-id="c3786-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="c3786-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Antik ismerje meg az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c3786-104">In this tutorial, you learn how toointegrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c3786-105">Antik további integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c3786-105">Integrating Blackboard Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c3786-106">Megadhatja a hozzáférés tooBlackboard további rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c3786-106">You can control in Azure AD who has access tooBlackboard Learn</span></span>
- <span data-ttu-id="c3786-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBlackboard (egyszeri bejelentkezés) további információ az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c3786-107">You can enable your users tooautomatically get signed-on tooBlackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c3786-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c3786-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c3786-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c3786-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3786-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c3786-110">Prerequisites</span></span>

<span data-ttu-id="c3786-111">tooconfigure Antik ismerje meg az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="c3786-111">tooconfigure Azure AD integration with Blackboard Learn, you need hello following items:</span></span>

- <span data-ttu-id="c3786-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c3786-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c3786-113">Egy Antik ismerje meg az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c3786-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c3786-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c3786-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c3786-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c3786-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c3786-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c3786-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c3786-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c3786-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c3786-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c3786-118">Scenario description</span></span>
<span data-ttu-id="c3786-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c3786-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c3786-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c3786-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c3786-121">További Antik hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c3786-121">Adding Blackboard Learn from hello gallery</span></span>
2. <span data-ttu-id="c3786-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c3786-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-hello-gallery"></a><span data-ttu-id="c3786-123">További Antik hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c3786-123">Adding Blackboard Learn from hello gallery</span></span>
<span data-ttu-id="c3786-124">tooconfigure hello integrációja Antik ismerje meg az Azure AD-be, meg kell tooadd Antik további hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c3786-124">tooconfigure hello integration of Blackboard Learn into Azure AD, you need tooadd Blackboard Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c3786-125">**tooadd Antik további hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c3786-125">**tooadd Blackboard Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c3786-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c3786-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c3786-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c3786-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c3786-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c3786-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c3786-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c3786-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c3786-133">Hello keresési mezőbe, írja be a **Antik további**.</span><span class="sxs-lookup"><span data-stu-id="c3786-133">In hello search box, type **Blackboard Learn**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="c3786-135">Hello eredmények panelen, jelölje ki a **Antik további**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c3786-135">In hello results panel, select **Blackboard Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c3786-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c3786-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c3786-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Antik megtudhatja, hogy a "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="c3786-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c3786-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow Antik megtudhatja, milyen hello megfelelőjére felhasználó tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c3786-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Blackboard Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="c3786-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Antik ismerje meg a hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="c3786-140">In other words, a link relationship between an Azure AD user and hello related user in Blackboard Learn needs toobe established.</span></span>

<span data-ttu-id="c3786-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Antik ismerje meg a.</span><span class="sxs-lookup"><span data-stu-id="c3786-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="c3786-142">tooconfigure és -teszthez az Azure AD az egyszeri bejelentkezés Antik ismerje meg, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c3786-142">tooconfigure and test Azure AD single sign-on with Blackboard Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c3786-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c3786-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c3786-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c3786-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c3786-145">**[Ismerje meg, antik tesztfelhasználó létrehozása](#creating-a-blackboard-learn-test-user)**  -toohave egy megfelelője a Britta Simon a Antik ismerje meg, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c3786-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - toohave a counterpart of Britta Simon in Blackboard Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c3786-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c3786-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c3786-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c3786-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c3786-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c3786-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c3786-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez Antik ismerje meg az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c3786-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="c3786-150">**az Azure AD tooconfigure egyszeri bejelentkezés Antik ismerje meg, a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c3786-150">**tooconfigure Azure AD single sign-on with Blackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="c3786-151">Az Azure portál, a hello hello **Antik további** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c3786-151">In hello Azure portal, on hello **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c3786-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c3786-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="c3786-155">A hello **Antik megtudhatja, tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c3786-155">On hello **Blackboard Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="c3786-157">a.</span><span class="sxs-lookup"><span data-stu-id="c3786-157">a.</span></span> <span data-ttu-id="c3786-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="c3786-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="c3786-159">b.</span><span class="sxs-lookup"><span data-stu-id="c3786-159">b.</span></span> <span data-ttu-id="c3786-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="c3786-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c3786-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c3786-161">These values are not real.</span></span> <span data-ttu-id="c3786-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="c3786-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c3786-163">Ügyfél [Antik további ügyfél-támogatási csoport](https://www.blackboard.com/support/index.aspx) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c3786-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) tooget these values.</span></span> 

4. <span data-ttu-id="c3786-164">Antik információk az alkalmazás egy meghatározott formátumban hello SAML helyességi feltételek vár.</span><span class="sxs-lookup"><span data-stu-id="c3786-164">Blackboard Learn application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c3786-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c3786-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="c3786-166">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="c3786-166">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="c3786-167">a következő képernyőkép hello kapcsolatos példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c3786-167">hello following screenshot shows an example about it.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="c3786-169">A hello **felhasználói attribútumok** lap **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútumok hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="c3786-169">In hello **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in hello image and perform hello following steps.</span></span> <span data-ttu-id="c3786-170">Hozzárendelt igazolnia hello Userprincipalname hello egyedi felhasználói itt attribútumaként, de toohello megfelelő értéket, amely egyértelműen azonosítja hello szervezet hello felhasználója és, amely leképezhető tooBlackboard további felhasználónév mező is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="c3786-170">We have mapped hello Userprincipalname as hello unique user attribute here but you can map it toohello appropriate value, which uniquely distinguishes hello user in hello organization and that maps tooBlackboard Learn username field.</span></span>
           
    | <span data-ttu-id="c3786-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c3786-171">Attribute Name</span></span> | <span data-ttu-id="c3786-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c3786-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="c3786-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="c3786-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="c3786-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="c3786-174">user.userprincipalname</span></span> |

    <span data-ttu-id="c3786-175">a.</span><span class="sxs-lookup"><span data-stu-id="c3786-175">a.</span></span> <span data-ttu-id="c3786-176">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c3786-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c3786-179">b.</span><span class="sxs-lookup"><span data-stu-id="c3786-179">b.</span></span> <span data-ttu-id="c3786-180">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="c3786-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="c3786-181">c.</span><span class="sxs-lookup"><span data-stu-id="c3786-181">c.</span></span> <span data-ttu-id="c3786-182">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="c3786-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c3786-183">d.</span><span class="sxs-lookup"><span data-stu-id="c3786-183">d.</span></span> <span data-ttu-id="c3786-184">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c3786-184">Click **Ok**.</span></span>

4. <span data-ttu-id="c3786-185">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c3786-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="c3786-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c3786-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c3786-189">A hello **Antik további konfigurációs** kattintson **Antik további konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c3786-189">On hello **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c3786-190">Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c3786-190">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="c3786-192">tooconfigure egyszeri bejelentkezést a **Antik további** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **SAML Entitásazonosító** túl[Antik ismerje meg, támogatja a](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3786-192">tooconfigure single sign-on on **Blackboard Learn** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID** too[Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="c3786-193">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c3786-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c3786-194">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c3786-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c3786-195">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c3786-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c3786-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3786-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="c3786-197">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c3786-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c3786-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c3786-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c3786-200">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c3786-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c3786-202">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c3786-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c3786-204">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c3786-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c3786-206">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c3786-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c3786-208">a.</span><span class="sxs-lookup"><span data-stu-id="c3786-208">a.</span></span> <span data-ttu-id="c3786-209">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c3786-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c3786-210">b.</span><span class="sxs-lookup"><span data-stu-id="c3786-210">b.</span></span> <span data-ttu-id="c3786-211">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c3786-211">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="c3786-212">c.</span><span class="sxs-lookup"><span data-stu-id="c3786-212">c.</span></span> <span data-ttu-id="c3786-213">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c3786-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c3786-214">d.</span><span class="sxs-lookup"><span data-stu-id="c3786-214">d.</span></span> <span data-ttu-id="c3786-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c3786-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="c3786-216">Ismerje meg, antik tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3786-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="c3786-217">Ebben a szakaszban Britta Simon nevű Antik ismerje meg a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c3786-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="c3786-218">Antik további információ az alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="c3786-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="c3786-219">Győződjön meg arról, hogy konfigurálta hello jogcímek hello szakaszban leírt módon  **[az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="c3786-219">Make sure that you have configured hello claims as described in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c3786-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c3786-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c3786-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBlackboard további információ megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c3786-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlackboard Learn.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c3786-223">**tooassign Britta Simon tooBlackboard további, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c3786-223">**tooassign Britta Simon tooBlackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="c3786-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c3786-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c3786-226">Hello alkalmazások listában válassza ki a **Antik további**.</span><span class="sxs-lookup"><span data-stu-id="c3786-226">In hello applications list, select **Blackboard Learn**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="c3786-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c3786-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c3786-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c3786-230">Click **Add** button.</span></span> <span data-ttu-id="c3786-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c3786-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c3786-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c3786-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c3786-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c3786-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c3786-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c3786-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c3786-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c3786-236">Testing single sign-on</span></span>

<span data-ttu-id="c3786-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c3786-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c3786-238">Hello Antik további csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Antik további alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c3786-238">When you click hello Blackboard Learn tile in hello Access Panel, you should get automatically signed-on tooyour Blackboard Learn application.</span></span> <span data-ttu-id="c3786-239">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c3786-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c3786-240">További források</span><span class="sxs-lookup"><span data-stu-id="c3786-240">Additional resources</span></span>

* [<span data-ttu-id="c3786-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c3786-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c3786-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c3786-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

