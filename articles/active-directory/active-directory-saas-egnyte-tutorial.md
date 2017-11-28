---
title: "Oktatóanyag: Azure Active Directoryval integrált Egnyte |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Egnyte között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="e07ff-103">Oktatóanyag: Azure Active Directoryval integrált Egnyte</span><span class="sxs-lookup"><span data-stu-id="e07ff-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="e07ff-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Egnyte az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e07ff-104">In this tutorial, you learn how toointegrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e07ff-105">Egnyte integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-105">Integrating Egnyte with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e07ff-106">Megadhatja a hozzáférés tooEgnyte rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e07ff-106">You can control in Azure AD who has access tooEgnyte</span></span>
- <span data-ttu-id="e07ff-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEgnyte (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e07ff-107">You can enable your users tooautomatically get signed-on tooEgnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e07ff-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e07ff-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e07ff-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e07ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e07ff-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e07ff-110">Prerequisites</span></span>

<span data-ttu-id="e07ff-111">az Azure AD integrálása Egnyte tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-111">tooconfigure Azure AD integration with Egnyte, you need hello following items:</span></span>

- <span data-ttu-id="e07ff-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e07ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e07ff-113">Egy Egnyte egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e07ff-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e07ff-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e07ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e07ff-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e07ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e07ff-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e07ff-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e07ff-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e07ff-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e07ff-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e07ff-118">Scenario description</span></span>
<span data-ttu-id="e07ff-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e07ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e07ff-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e07ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e07ff-121">Hello gyűjteményből Egnyte hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e07ff-121">Adding Egnyte from hello gallery</span></span>
2. <span data-ttu-id="e07ff-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e07ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-hello-gallery"></a><span data-ttu-id="e07ff-123">Hello gyűjteményből Egnyte hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e07ff-123">Adding Egnyte from hello gallery</span></span>
<span data-ttu-id="e07ff-124">tooconfigure hello integrációja Egnyte az Azure AD-be, meg kell tooadd Egnyte hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e07ff-124">tooconfigure hello integration of Egnyte into Azure AD, you need tooadd Egnyte from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e07ff-125">**tooadd Egnyte hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e07ff-125">**tooadd Egnyte from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e07ff-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e07ff-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e07ff-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e07ff-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e07ff-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e07ff-133">Hello keresési mezőbe, írja be a **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-133">In hello search box, type **Egnyte**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="e07ff-135">A hello eredmények panelen válassza ki a **Egnyte**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-135">In hello results panel, select **Egnyte**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e07ff-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e07ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e07ff-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Egnyte</span><span class="sxs-lookup"><span data-stu-id="e07ff-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e07ff-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Egnyte tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e07ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Egnyte is tooa user in Azure AD.</span></span> <span data-ttu-id="e07ff-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Egnyte közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e07ff-140">In other words, a link relationship between an Azure AD user and hello related user in Egnyte needs toobe established.</span></span>

<span data-ttu-id="e07ff-141">Egnyte, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e07ff-141">In Egnyte, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e07ff-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Egnyte-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e07ff-142">tooconfigure and test Azure AD single sign-on with Egnyte, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e07ff-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e07ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e07ff-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e07ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e07ff-145">**[Egy Egnyte tesztfelhasználó létrehozása](#creating-an-egnyte-test-user)**  -toohave egy megfelelője a Britta Simon a Egnyte, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e07ff-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - toohave a counterpart of Britta Simon in Egnyte that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e07ff-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e07ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e07ff-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e07ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e07ff-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e07ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e07ff-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Egnyte alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e07ff-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="e07ff-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Egnyte, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e07ff-150">**tooconfigure Azure AD single sign-on with Egnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="e07ff-151">Az Azure portál, a hello hello **Egnyte** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-151">In hello Azure portal, on hello **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e07ff-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e07ff-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="e07ff-155">A hello **Egnyte tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-155">On hello **Egnyte Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="e07ff-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="e07ff-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e07ff-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e07ff-158">This value is not real.</span></span> <span data-ttu-id="e07ff-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="e07ff-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="e07ff-160">Ügyfél [Egnyte ügyfél-támogatási csoport](https://www.egnyte.com/corp/contact_egnyte.html) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="e07ff-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="e07ff-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e07ff-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="e07ff-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e07ff-165">A hello **Egnyte konfigurációs** kattintson **konfigurálása Egnyte** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e07ff-165">On hello **Egnyte Configuration** section, click **Configure Egnyte** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e07ff-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e07ff-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="e07ff-168">Egy másik webes böngészőablakban jelentkezzen tooyour Egnyte vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e07ff-168">In a different web browser window, log in tooyour Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="e07ff-169">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="e07ff-170">![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787819.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="e07ff-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="e07ff-171">A hello kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-171">In hello menu, click **Settings**.</span></span>

   <span data-ttu-id="e07ff-172">![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787820.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="e07ff-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="e07ff-173">Kattintson a hello **konfigurációs** fülre, majd **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-173">Click hello **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="e07ff-174">![Biztonsági](./media/active-directory-saas-egnyte-tutorial/ic787821.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="e07ff-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="e07ff-175">A hello **egyszeri bejelentkezés hitelesítési** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-175">In hello **Single Sign-On Authentication** section, perform hello following steps:</span></span>

    <span data-ttu-id="e07ff-176">![Egyszeri bejelentkezés hitelesítési](./media/active-directory-saas-egnyte-tutorial/ic787822.png "egyszeri bejelentkezés a hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="e07ff-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="e07ff-177">a.</span><span class="sxs-lookup"><span data-stu-id="e07ff-177">a.</span></span> <span data-ttu-id="e07ff-178">Mint **egyszeri bejelentkezés hitelesítési**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="e07ff-179">b.</span><span class="sxs-lookup"><span data-stu-id="e07ff-179">b.</span></span> <span data-ttu-id="e07ff-180">Mint **identitásszolgáltató**, jelölje be **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="e07ff-181">c.</span><span class="sxs-lookup"><span data-stu-id="e07ff-181">c.</span></span> <span data-ttu-id="e07ff-182">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** hello Azure-portálon másol **Identity provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e07ff-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into hello **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="e07ff-183">d.</span><span class="sxs-lookup"><span data-stu-id="e07ff-183">d.</span></span> <span data-ttu-id="e07ff-184">Beillesztés **SAML Entitásazonosító** amely hello az Azure-portálon másolta **Identity provider Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e07ff-184">Paste **SAML Entity ID** which you have copied from Azure portal into hello **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="e07ff-185">e.</span><span class="sxs-lookup"><span data-stu-id="e07ff-185">e.</span></span> <span data-ttu-id="e07ff-186">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **szolgáltató identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e07ff-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="e07ff-187">f.</span><span class="sxs-lookup"><span data-stu-id="e07ff-187">f.</span></span> <span data-ttu-id="e07ff-188">Mint **alapértelmezett felhasználói hozzárendelésének**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="e07ff-189">g.</span><span class="sxs-lookup"><span data-stu-id="e07ff-189">g.</span></span> <span data-ttu-id="e07ff-190">Mint **tartományspecifikus kibocsátó érték**, jelölje be **le van tiltva**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="e07ff-191">h.</span><span class="sxs-lookup"><span data-stu-id="e07ff-191">h.</span></span> <span data-ttu-id="e07ff-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e07ff-193">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e07ff-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e07ff-194">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e07ff-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e07ff-195">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e07ff-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e07ff-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e07ff-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="e07ff-197">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e07ff-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e07ff-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e07ff-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e07ff-200">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e07ff-202">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e07ff-204">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e07ff-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e07ff-206">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e07ff-208">a.</span><span class="sxs-lookup"><span data-stu-id="e07ff-208">a.</span></span> <span data-ttu-id="e07ff-209">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e07ff-210">b.</span><span class="sxs-lookup"><span data-stu-id="e07ff-210">b.</span></span> <span data-ttu-id="e07ff-211">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e07ff-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e07ff-212">c.</span><span class="sxs-lookup"><span data-stu-id="e07ff-212">c.</span></span> <span data-ttu-id="e07ff-213">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e07ff-214">d.</span><span class="sxs-lookup"><span data-stu-id="e07ff-214">d.</span></span> <span data-ttu-id="e07ff-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="e07ff-216">Egy Egnyte tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e07ff-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="e07ff-217">az Azure AD tooenable felhasználók toolog a tooEgnyte, akkor ki kell építenie Egnyte be.</span><span class="sxs-lookup"><span data-stu-id="e07ff-217">tooenable Azure AD users toolog in tooEgnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="e07ff-218">Egnyte hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e07ff-218">In hello case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="e07ff-219">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e07ff-219">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="e07ff-220">Jelentkezzen be tooyour **Egnyte** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e07ff-220">Log in tooyour **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="e07ff-221">Nyissa meg túl**beállítások \> felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-221">Go too**Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="e07ff-222">Kattintson a **új felhasználó hozzáadása**, majd válassza ki a hello típus tooadd kívánt felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e07ff-222">Click **Add New User**, and then select hello type of user you want tooadd.</span></span>
   
   <span data-ttu-id="e07ff-223">![Felhasználók](./media/active-directory-saas-egnyte-tutorial/ic787824.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="e07ff-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="e07ff-224">A hello **új általános jogú felhasználó** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e07ff-224">In hello **New Standard User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="e07ff-225">![Új általános jogú felhasználó](./media/active-directory-saas-egnyte-tutorial/ic787825.png "új általános jogú felhasználó")</span><span class="sxs-lookup"><span data-stu-id="e07ff-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="e07ff-226">a.</span><span class="sxs-lookup"><span data-stu-id="e07ff-226">a.</span></span> <span data-ttu-id="e07ff-227">Típus hello **E-mail**, **felhasználónév**, és egyéb részleteket a egy érvényes Azure Active Directory-fióknevet, amelyet tooprovision.</span><span class="sxs-lookup"><span data-stu-id="e07ff-227">Type hello **Email**, **Username**, and other details of a valid Azure Active Directory account you want tooprovision.</span></span>
   
   <span data-ttu-id="e07ff-228">b.</span><span class="sxs-lookup"><span data-stu-id="e07ff-228">b.</span></span> <span data-ttu-id="e07ff-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="e07ff-230">hello Azure Active Directory fióktulajdonos e-mailben értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="e07ff-230">hello Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="e07ff-231">Bármely más Egnyte felhasználói fiók létrehozása eszközök vagy Egnyte tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e07ff-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e07ff-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e07ff-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e07ff-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEgnyte megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e07ff-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEgnyte.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e07ff-235">**tooassign Britta Simon tooEgnyte, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e07ff-235">**tooassign Britta Simon tooEgnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="e07ff-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e07ff-238">Hello alkalmazások listában válassza ki a **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-238">In hello applications list, select **Egnyte**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="e07ff-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e07ff-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e07ff-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e07ff-242">Click **Add** button.</span></span> <span data-ttu-id="e07ff-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e07ff-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e07ff-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e07ff-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e07ff-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e07ff-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e07ff-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e07ff-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e07ff-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e07ff-248">Testing single sign-on</span></span>

<span data-ttu-id="e07ff-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e07ff-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e07ff-250">Ha a hozzáférési Panel hello hello Egnyte csempe gombra kattint, automatikusan bejelentkezett tooyour Egnyte alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e07ff-250">When you click hello Egnyte tile in hello Access Panel, you should get automatically signed-on tooyour Egnyte application.</span></span>
<span data-ttu-id="e07ff-251">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e07ff-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e07ff-252">További források</span><span class="sxs-lookup"><span data-stu-id="e07ff-252">Additional resources</span></span>

* [<span data-ttu-id="e07ff-253">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e07ff-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e07ff-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e07ff-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

