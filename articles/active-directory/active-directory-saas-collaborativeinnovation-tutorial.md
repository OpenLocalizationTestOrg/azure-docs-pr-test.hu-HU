---
title: "Oktatóanyag: Azure Active Directoryval integrált együttműködési innováció |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az együttműködési innováció között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e85fabfe11a380129f319a101aa7c7a9491260f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="4a2ff-103">Oktatóanyag: Azure Active Directoryval integrált együttműködési innováció</span><span class="sxs-lookup"><span data-stu-id="4a2ff-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="4a2ff-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate együttműködési innováció az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4a2ff-104">In this tutorial, you learn how toointegrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a2ff-105">Együttműködési innováció integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-105">Integrating Collaborative Innovation with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4a2ff-106">Megadhatja a hozzáférés tooCollaborative innováció rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4a2ff-106">You can control in Azure AD who has access tooCollaborative Innovation</span></span>
- <span data-ttu-id="4a2ff-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCollaborative innováció (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4a2ff-107">You can enable your users tooautomatically get signed-on tooCollaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4a2ff-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4a2ff-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4a2ff-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a2ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a2ff-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4a2ff-110">Prerequisites</span></span>

<span data-ttu-id="4a2ff-111">tooconfigure együttműködési innováció az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-111">tooconfigure Azure AD integration with Collaborative Innovation, you need hello following items:</span></span>

- <span data-ttu-id="4a2ff-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4a2ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a2ff-113">A együttműködési innováció egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="4a2ff-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a2ff-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a2ff-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a2ff-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a2ff-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a2ff-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a2ff-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4a2ff-118">Scenario description</span></span>
<span data-ttu-id="4a2ff-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a2ff-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a2ff-121">Együttműködési innováció hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a2ff-121">Adding Collaborative Innovation from hello gallery</span></span>
2. <span data-ttu-id="4a2ff-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a2ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-hello-gallery"></a><span data-ttu-id="4a2ff-123">Együttműködési innováció hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a2ff-123">Adding Collaborative Innovation from hello gallery</span></span>
<span data-ttu-id="4a2ff-124">tooconfigure hello integráció az együttműködést innováció, az Azure AD-be, meg kell tooadd együttműködési innováció hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-124">tooconfigure hello integration of Collaborative Innovation into Azure AD, you need tooadd Collaborative Innovation from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4a2ff-125">**tooadd együttműködési innováció hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a2ff-125">**tooadd Collaborative Innovation from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a2ff-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4a2ff-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4a2ff-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4a2ff-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4a2ff-133">Hello keresési mezőbe, írja be a **együttműködési innováció**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-133">In hello search box, type **Collaborative Innovation**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="4a2ff-135">A hello eredmények panelen válassza a **együttműködési innováció**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-135">In hello results panel, select **Collaborative Innovation**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4a2ff-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a2ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4a2ff-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az együttműködési innováció "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="4a2ff-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4a2ff-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó együttműködési innováció tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Collaborative Innovation is tooa user in Azure AD.</span></span> <span data-ttu-id="4a2ff-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello együttműködési innováció közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-140">In other words, a link relationship between an Azure AD user and hello related user in Collaborative Innovation needs toobe established.</span></span>

<span data-ttu-id="4a2ff-141">Együttműködési innováció, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-141">In Collaborative Innovation, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4a2ff-142">tooconfigure és az együttműködési innováció az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-142">tooconfigure and test Azure AD single sign-on with Collaborative Innovation, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4a2ff-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4a2ff-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a2ff-145">**[Együttműködési innováció tesztfelhasználó létrehozása](#creating-a-collaborative-innovation-test-user)**  -toohave egy megfelelője a Britta Simon az együttműködést innováció, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - toohave a counterpart of Britta Simon in Collaborative Innovation that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4a2ff-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a2ff-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4a2ff-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a2ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4a2ff-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az együttműködési innováció alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="4a2ff-150">**az Azure AD tooconfigure egyszeri bejelentkezést az együttműködési innováció, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a2ff-150">**tooconfigure Azure AD single sign-on with Collaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a2ff-151">Az Azure portál, a hello hello **együttműködési innováció** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-151">In hello Azure portal, on hello **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4a2ff-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="4a2ff-155">A hello **együttműködési innováció tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-155">On hello **Collaborative Innovation Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="4a2ff-157">a.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-157">a.</span></span> <span data-ttu-id="4a2ff-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="4a2ff-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="4a2ff-159">b.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-159">b.</span></span> <span data-ttu-id="4a2ff-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="4a2ff-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="4a2ff-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-161">These values are not real.</span></span> <span data-ttu-id="4a2ff-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4a2ff-163">Ügyfél [együttműködési innováció ügyfél-támogatási csoport](https://www.unilever.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) tooget these values.</span></span>  

4. <span data-ttu-id="4a2ff-164">Együttműködési innováció alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-164">Collaborative Innovation application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="4a2ff-165">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="4a2ff-166">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="4a2ff-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-167">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="4a2ff-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** hello jelölőnégyzet **felhasználói attribútumok** tooexpand hello attribútumok szakasz.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="4a2ff-170">Hajtsa végre a következő lépéseket mindegyik megjelenített hello attribútumok – hello</span><span class="sxs-lookup"><span data-stu-id="4a2ff-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="4a2ff-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="4a2ff-171">Attribute Name</span></span> | <span data-ttu-id="4a2ff-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="4a2ff-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="4a2ff-173">givenName</span><span class="sxs-lookup"><span data-stu-id="4a2ff-173">givenname</span></span> | <span data-ttu-id="4a2ff-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="4a2ff-174">user.givenname</span></span> |
    | <span data-ttu-id="4a2ff-175">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="4a2ff-175">surname</span></span> | <span data-ttu-id="4a2ff-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="4a2ff-176">user.surname</span></span> |
    | <span data-ttu-id="4a2ff-177">e-mail cím</span><span class="sxs-lookup"><span data-stu-id="4a2ff-177">emailaddress</span></span> | <span data-ttu-id="4a2ff-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="4a2ff-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="4a2ff-179">név</span><span class="sxs-lookup"><span data-stu-id="4a2ff-179">name</span></span> | <span data-ttu-id="4a2ff-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="4a2ff-180">user.userprincipalname</span></span> |

    <span data-ttu-id="4a2ff-181">a.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-181">a.</span></span> <span data-ttu-id="4a2ff-182">Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-182">Click hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="4a2ff-184">b.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-184">b.</span></span> <span data-ttu-id="4a2ff-185">Hello URL-érték törlése hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="4a2ff-186">c.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-186">c.</span></span> <span data-ttu-id="4a2ff-187">Kattintson a **Ok** toosave hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-187">Click **Ok** toosave hello setting.</span></span>

6. <span data-ttu-id="4a2ff-188">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-188">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="4a2ff-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="4a2ff-192">tooconfigure egyszeri bejelentkezést a **együttműködési innováció** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[együttműködési innováció támogatási csoport](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="4a2ff-192">tooconfigure single sign-on on **Collaborative Innovation** side, you need toosend hello downloaded **Metadata XML** too[Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="4a2ff-193">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="4a2ff-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4a2ff-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4a2ff-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4a2ff-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a2ff-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4a2ff-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a2ff-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="4a2ff-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4a2ff-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4a2ff-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a2ff-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a2ff-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a2ff-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a2ff-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a2ff-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4a2ff-209">a.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-209">a.</span></span> <span data-ttu-id="4a2ff-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a2ff-211">b.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-211">b.</span></span> <span data-ttu-id="4a2ff-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4a2ff-213">c.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-213">c.</span></span> <span data-ttu-id="4a2ff-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4a2ff-215">d.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-215">d.</span></span> <span data-ttu-id="4a2ff-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="4a2ff-217">Együttműködési innováció tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a2ff-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="4a2ff-218">az Azure AD tooenable felhasználók toolog tooCollaborative innováció, a ezeket ki kell építenie az együttműködést innováció.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-218">tooenable Azure AD users toolog in tooCollaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="4a2ff-219">Ez az alkalmazás esetén automatikus hello alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-219">In case of this application provisioning is automatic as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="4a2ff-220">Így nincs nincs szükség tooperform itt végre.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-220">So there is no need tooperform any steps here.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4a2ff-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4a2ff-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4a2ff-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCollaborative innováció megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCollaborative Innovation.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4a2ff-224">**tooassign Britta Simon tooCollaborative innováció, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4a2ff-224">**tooassign Britta Simon tooCollaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a2ff-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4a2ff-227">Hello alkalmazások listában válassza ki a **együttműködési innováció**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-227">In hello applications list, select **Collaborative Innovation**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="4a2ff-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4a2ff-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-231">Click **Add** button.</span></span> <span data-ttu-id="4a2ff-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4a2ff-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4a2ff-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a2ff-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4a2ff-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4a2ff-237">Testing single sign-on</span></span>

<span data-ttu-id="4a2ff-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4a2ff-239">Ha hello együttműködési innováció csempe a hozzáférési Panel hello gombra kattint, szerezheti be együttműködési innováció alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="4a2ff-239">When you click hello Collaborative Innovation tile in hello Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="4a2ff-240">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4a2ff-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4a2ff-241">További források</span><span class="sxs-lookup"><span data-stu-id="4a2ff-241">Additional resources</span></span>

* [<span data-ttu-id="4a2ff-242">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4a2ff-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a2ff-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4a2ff-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

