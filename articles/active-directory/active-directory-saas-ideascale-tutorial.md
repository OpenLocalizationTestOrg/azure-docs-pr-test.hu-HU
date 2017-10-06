---
title: "Oktatóanyag: Azure Active Directoryval integrált IdeaScale |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és IdeaScale között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a><span data-ttu-id="18951-103">Oktatóanyag: Azure Active Directoryval integrált IdeaScale</span><span class="sxs-lookup"><span data-stu-id="18951-103">Tutorial: Azure Active Directory integration with IdeaScale</span></span>

<span data-ttu-id="18951-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate IdeaScale az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="18951-104">In this tutorial, you learn how toointegrate IdeaScale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="18951-105">IdeaScale integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="18951-105">Integrating IdeaScale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="18951-106">Megadhatja a hozzáférés tooIdeaScale rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="18951-106">You can control in Azure AD who has access tooIdeaScale</span></span>
- <span data-ttu-id="18951-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIdeaScale (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="18951-107">You can enable your users tooautomatically get signed-on tooIdeaScale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="18951-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="18951-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="18951-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="18951-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18951-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="18951-110">Prerequisites</span></span>

<span data-ttu-id="18951-111">az Azure AD integrálása IdeaScale tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="18951-111">tooconfigure Azure AD integration with IdeaScale, you need hello following items:</span></span>

- <span data-ttu-id="18951-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="18951-112">An Azure AD subscription</span></span>
- <span data-ttu-id="18951-113">Egy IdeaScale egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="18951-113">An IdeaScale single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="18951-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="18951-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="18951-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="18951-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="18951-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="18951-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="18951-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="18951-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="18951-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="18951-118">Scenario description</span></span>
<span data-ttu-id="18951-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="18951-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="18951-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="18951-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="18951-121">Hello gyűjteményből IdeaScale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18951-121">Adding IdeaScale from hello gallery</span></span>
2. <span data-ttu-id="18951-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="18951-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ideascale-from-hello-gallery"></a><span data-ttu-id="18951-123">Hello gyűjteményből IdeaScale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18951-123">Adding IdeaScale from hello gallery</span></span>
<span data-ttu-id="18951-124">tooconfigure hello integrációja IdeaScale az Azure AD-be, meg kell tooadd IdeaScale hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="18951-124">tooconfigure hello integration of IdeaScale into Azure AD, you need tooadd IdeaScale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="18951-125">**tooadd IdeaScale hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="18951-125">**tooadd IdeaScale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="18951-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="18951-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="18951-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="18951-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="18951-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="18951-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="18951-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="18951-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="18951-133">Hello keresési mezőbe, írja be a **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="18951-133">In hello search box, type **IdeaScale**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. <span data-ttu-id="18951-135">A hello eredmények panelen válassza ki a **IdeaScale**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="18951-135">In hello results panel, select **IdeaScale**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="18951-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="18951-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="18951-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján IdeaScale</span><span class="sxs-lookup"><span data-stu-id="18951-138">In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="18951-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó IdeaScale tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="18951-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IdeaScale is tooa user in Azure AD.</span></span> <span data-ttu-id="18951-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello IdeaScale közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="18951-140">In other words, a link relationship between an Azure AD user and hello related user in IdeaScale needs toobe established.</span></span>

<span data-ttu-id="18951-141">IdeaScale, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="18951-141">In IdeaScale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="18951-142">tooconfigure és az Azure AD az egyszeri bejelentkezés IdeaScale-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="18951-142">tooconfigure and test Azure AD single sign-on with IdeaScale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="18951-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="18951-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="18951-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18951-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="18951-145">**[Egy IdeaScale tesztfelhasználó létrehozása](#creating-an-ideascale-test-user)**  -toohave egy megfelelője a Britta Simon a IdeaScale, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="18951-145">**[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - toohave a counterpart of Britta Simon in IdeaScale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="18951-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="18951-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="18951-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="18951-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="18951-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="18951-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="18951-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az IdeaScale alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="18951-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IdeaScale application.</span></span>

<span data-ttu-id="18951-150">**az Azure AD tooconfigure egyszeri bejelentkezést a IdeaScale, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="18951-150">**tooconfigure Azure AD single sign-on with IdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="18951-151">Az Azure portál, a hello hello **IdeaScale** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="18951-151">In hello Azure portal, on hello **IdeaScale** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="18951-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="18951-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. <span data-ttu-id="18951-155">A hello **IdeaScale tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18951-155">On hello **IdeaScale Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    <span data-ttu-id="18951-157">a.</span><span class="sxs-lookup"><span data-stu-id="18951-157">a.</span></span> <span data-ttu-id="18951-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.ideascale.com`</span><span class="sxs-lookup"><span data-stu-id="18951-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.ideascale.com`</span></span>

    <span data-ttu-id="18951-159">b.</span><span class="sxs-lookup"><span data-stu-id="18951-159">b.</span></span> <span data-ttu-id="18951-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="18951-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > <span data-ttu-id="18951-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="18951-161">These values are not real.</span></span> <span data-ttu-id="18951-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="18951-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="18951-163">Ügyfél [IdeaScale ügyfél-támogatási csoport](http://support.ideascale.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="18951-163">Contact [IdeaScale Client support team](http://support.ideascale.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="18951-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="18951-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. <span data-ttu-id="18951-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="18951-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="18951-168">A hello **IdeaScale konfigurációs** kattintson **konfigurálása IdeaScale** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="18951-168">On hello **IdeaScale Configuration** section, click **Configure IdeaScale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="18951-169">Másolás hello **Sign-Out URL-címet, és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz**.</span><span class="sxs-lookup"><span data-stu-id="18951-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. <span data-ttu-id="18951-171">Egy másik webes böngészőablakban jelentkezzen tooyour IdeaScale vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="18951-171">In a different web browser window, log in tooyour IdeaScale company site as an administrator.</span></span>

8. <span data-ttu-id="18951-172">Nyissa meg túl**közösségi beállítások**.</span><span class="sxs-lookup"><span data-stu-id="18951-172">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="18951-173">![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")</span><span class="sxs-lookup"><span data-stu-id="18951-173">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

9. <span data-ttu-id="18951-174">Nyissa meg túl**biztonsági \> egyszeri bejelentkezés beállítások**.</span><span class="sxs-lookup"><span data-stu-id="18951-174">Go too**Security \> Single Signon Settings**.</span></span>
   
    <span data-ttu-id="18951-175">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790848.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="18951-175">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Single Signon Settings")</span></span>

10. <span data-ttu-id="18951-176">Mint **egyszeri-bejelentkezés típusa**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="18951-176">As **Single-Signon Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="18951-177">![Az egyszeri bejelentkezés típusa](./media/active-directory-saas-ideascale-tutorial/ic790849.png "az egyszeri bejelentkezés típusa")</span><span class="sxs-lookup"><span data-stu-id="18951-177">![Single Signon Type](./media/active-directory-saas-ideascale-tutorial/ic790849.png "Single Signon Type")</span></span>

11. <span data-ttu-id="18951-178">A hello **egyszeri bejelentkezés beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18951-178">On hello **Single Signon Settings** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="18951-179">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790850.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="18951-179">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Single Signon Settings")</span></span>
   
    <span data-ttu-id="18951-180">a.</span><span class="sxs-lookup"><span data-stu-id="18951-180">a.</span></span> <span data-ttu-id="18951-181">A **SAML IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="18951-181">In **SAML IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="18951-182">b.</span><span class="sxs-lookup"><span data-stu-id="18951-182">b.</span></span> <span data-ttu-id="18951-183">A letöltött metaadatfájl hello tartalmának másolása Azure-portálon, és illessze be hello **SAML IdP metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="18951-183">Copy hello content of your downloaded metadata file from Azure portal, and paste it into hello **SAML IdP Metadata** textbox.</span></span>

    <span data-ttu-id="18951-184">c.</span><span class="sxs-lookup"><span data-stu-id="18951-184">c.</span></span> <span data-ttu-id="18951-185">A **kijelentkezési sikeres URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="18951-185">In **Logout Success URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="18951-186">d.</span><span class="sxs-lookup"><span data-stu-id="18951-186">d.</span></span> <span data-ttu-id="18951-187">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="18951-187">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="18951-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="18951-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="18951-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="18951-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="18951-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="18951-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="18951-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="18951-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="18951-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="18951-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="18951-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="18951-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="18951-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="18951-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="18951-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="18951-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="18951-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18951-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="18951-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18951-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="18951-203">a.</span><span class="sxs-lookup"><span data-stu-id="18951-203">a.</span></span> <span data-ttu-id="18951-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="18951-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="18951-205">b.</span><span class="sxs-lookup"><span data-stu-id="18951-205">b.</span></span> <span data-ttu-id="18951-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="18951-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="18951-207">c.</span><span class="sxs-lookup"><span data-stu-id="18951-207">c.</span></span> <span data-ttu-id="18951-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="18951-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="18951-209">d.</span><span class="sxs-lookup"><span data-stu-id="18951-209">d.</span></span> <span data-ttu-id="18951-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="18951-210">Click **Create**.</span></span>
 
### <a name="creating-an-ideascale-test-user"></a><span data-ttu-id="18951-211">Egy IdeaScale tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="18951-211">Creating an IdeaScale test user</span></span>

<span data-ttu-id="18951-212">az Azure AD tooenable felhasználók toolog történő IdeaScale, azok ki kell építenie a tooIdeaScale.</span><span class="sxs-lookup"><span data-stu-id="18951-212">tooenable Azure AD users toolog into IdeaScale, they must be provisioned in tooIdeaScale.</span></span> <span data-ttu-id="18951-213">IdeaScale hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="18951-213">In hello case of IdeaScale, provisioning is a manual task.</span></span>

<span data-ttu-id="18951-214">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="18951-214">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="18951-215">Jelentkezzen be tooyour **IdeaScale** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="18951-215">Log in tooyour **IdeaScale** company site as administrator.</span></span>

2. <span data-ttu-id="18951-216">Nyissa meg túl**közösségi beállítások**.</span><span class="sxs-lookup"><span data-stu-id="18951-216">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="18951-217">![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")</span><span class="sxs-lookup"><span data-stu-id="18951-217">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

3. <span data-ttu-id="18951-218">Nyissa meg túl**alapbeállítások \> tag felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="18951-218">Go too**Basic Settings \> Member Management**.</span></span>

4. <span data-ttu-id="18951-219">Kattintson a **felvenni abban az esetben**.</span><span class="sxs-lookup"><span data-stu-id="18951-219">Click **Add Member**.</span></span>
   
    <span data-ttu-id="18951-220">![Tag felügyeleti](./media/active-directory-saas-ideascale-tutorial/ic790852.png "tag felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="18951-220">![Member Management](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Member Management")</span></span>

5. <span data-ttu-id="18951-221">Az új tag hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="18951-221">In hello Add New Member section, perform hello following steps:</span></span>
   
    <span data-ttu-id="18951-222">![Adja hozzá az új tag](./media/active-directory-saas-ideascale-tutorial/ic790853.png "új tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="18951-222">![Add New Member](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Add New Member")</span></span>
   
    <span data-ttu-id="18951-223">a.</span><span class="sxs-lookup"><span data-stu-id="18951-223">a.</span></span> <span data-ttu-id="18951-224">A hello **E-mail címek** szövegmezőhöz típus hello e-mail címe kívánt tooprovision érvényes AAD-fiókba.</span><span class="sxs-lookup"><span data-stu-id="18951-224">In hello **Email Addresses** textbox, type hello email address of a valid AAD account you want tooprovision.</span></span>
   
    <span data-ttu-id="18951-225">b.</span><span class="sxs-lookup"><span data-stu-id="18951-225">b.</span></span> <span data-ttu-id="18951-226">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="18951-226">Click **Save Changes**.</span></span> 
   
    >[!NOTE]
    ><span data-ttu-id="18951-227">hello Azure Active Directory fióktulajdonos lekérdezi az e-mailben található egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="18951-227">hello Azure Active Directory account holder gets an email with a link tooconfirm hello account before it becomes active.</span></span>
      
>[!NOTE]
><span data-ttu-id="18951-228">Bármely más IdeaScale felhasználói fiók létrehozása eszközök vagy IdeaScale tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="18951-228">You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale tooprovision AAD user accounts.</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="18951-229">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="18951-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="18951-230">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIdeaScale megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="18951-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIdeaScale.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="18951-232">**tooassign Britta Simon tooIdeaScale, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="18951-232">**tooassign Britta Simon tooIdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="18951-233">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="18951-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="18951-235">Hello alkalmazások listában válassza ki a **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="18951-235">In hello applications list, select **IdeaScale**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. <span data-ttu-id="18951-237">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="18951-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="18951-239">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="18951-239">Click **Add** button.</span></span> <span data-ttu-id="18951-240">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18951-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="18951-242">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="18951-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="18951-243">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18951-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="18951-244">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18951-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="18951-245">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="18951-245">Testing single sign-on</span></span>


<span data-ttu-id="18951-246">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="18951-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="18951-247">Ha a hozzáférési Panel hello hello IdeaScale csempe gombra kattint, automatikusan bejelentkezett tooyour IdeaScale alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="18951-247">When you click hello IdeaScale tile in hello Access Panel, you should get automatically signed-on tooyour IdeaScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18951-248">További források</span><span class="sxs-lookup"><span data-stu-id="18951-248">Additional resources</span></span>

* [<span data-ttu-id="18951-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="18951-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="18951-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="18951-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

