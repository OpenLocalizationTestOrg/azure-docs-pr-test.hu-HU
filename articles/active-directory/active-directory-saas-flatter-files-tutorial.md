---
title: "Oktatóanyag: Azure Active Directory-integráció laposabb fájlokkal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és laposabb fájlok között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 73ca2613b7bbaf9992ecf624ff5defabaa44f7a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="4b7c8-103">Oktatóanyag: Azure Active Directoryval integrált laposabb fájlok</span><span class="sxs-lookup"><span data-stu-id="4b7c8-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="4b7c8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate laposabb fájlok az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4b7c8-104">In this tutorial, you learn how toointegrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4b7c8-105">Laposabb fájlok integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-105">Integrating Flatter Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4b7c8-106">Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-ben tooFlatter fájlok</span><span class="sxs-lookup"><span data-stu-id="4b7c8-106">You can control in Azure AD who has access tooFlatter Files</span></span>
- <span data-ttu-id="4b7c8-107">Engedélyezheti a felhasználók tooautomatically bejelentkezett tooFlatter fájlok (egyszeri bejelentkezés) az Azure AD-fiókok az beszerzése</span><span class="sxs-lookup"><span data-stu-id="4b7c8-107">You can enable your users tooautomatically get signed-on tooFlatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4b7c8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4b7c8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4b7c8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4b7c8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b7c8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4b7c8-110">Prerequisites</span></span>

<span data-ttu-id="4b7c8-111">tooconfigure laposabb fájlok az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-111">tooconfigure Azure AD integration with Flatter Files, you need hello following items:</span></span>

- <span data-ttu-id="4b7c8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4b7c8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4b7c8-113">Egy laposabb fájlok egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4b7c8-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4b7c8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4b7c8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4b7c8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4b7c8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4b7c8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4b7c8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-118">Scenario description</span></span>
<span data-ttu-id="4b7c8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4b7c8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4b7c8-121">Hello gyűjteményből laposabb fájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-121">Adding Flatter Files from hello gallery</span></span>
2. <span data-ttu-id="4b7c8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4b7c8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-hello-gallery"></a><span data-ttu-id="4b7c8-123">Hello gyűjteményből laposabb fájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-123">Adding Flatter Files from hello gallery</span></span>
<span data-ttu-id="4b7c8-124">tooconfigure hello integrációs laposabb fájlok az Azure AD-be, meg kell tooadd laposabb fájlok hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-124">tooconfigure hello integration of Flatter Files into Azure AD, you need tooadd Flatter Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4b7c8-125">**tooadd laposabb fájlok hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-125">**tooadd Flatter Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b7c8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4b7c8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4b7c8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4b7c8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4b7c8-133">Hello keresési mezőbe, írja be a **laposabb fájlok**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-133">In hello search box, type **Flatter Files**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="4b7c8-135">A hello eredmények panelen válassza ki a **laposabb fájlok**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-135">In hello results panel, select **Flatter Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4b7c8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4b7c8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4b7c8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján laposabb fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4b7c8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói laposabb fájlokban tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Flatter Files is tooa user in Azure AD.</span></span> <span data-ttu-id="4b7c8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello laposabb fájlok közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-140">In other words, a link relationship between an Azure AD user and hello related user in Flatter Files needs toobe established.</span></span>

<span data-ttu-id="4b7c8-141">Laposabb fájlokban, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-141">In Flatter Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4b7c8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése laposabb fájlokkal, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-142">tooconfigure and test Azure AD single sign-on with Flatter Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4b7c8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4b7c8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4b7c8-145">**[Laposabb fájlok tesztfelhasználó létrehozása](#creating-a-flatter-files-test-user)**  -toohave egy megfelelője a Britta Simon csatolt toohello az Azure AD felhasználói ábrázolása laposabb fájlban.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - toohave a counterpart of Britta Simon in Flatter Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4b7c8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4b7c8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4b7c8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4b7c8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az laposabb fájlok alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="4b7c8-150">**az Azure AD tooconfigure egyszeri bejelentkezés laposabb fájlokkal hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-150">**tooconfigure Azure AD single sign-on with Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b7c8-151">Az Azure portál, a hello hello **laposabb fájlok** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-151">In hello Azure portal, on hello **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4b7c8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="4b7c8-155">A hello **laposabb fájlok tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-155">On hello **Flatter Files Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="4b7c8-157">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="4b7c8-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4b7c8-161">A hello **laposabb fájlok konfigurációs** kattintson **laposabb fájlok konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-161">On hello **Flatter Files Configuration** section, click **Configure Flatter Files** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4b7c8-162">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-162">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="4b7c8-164">Bejelentkezés tooyour laposabb fájlok alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-164">Sign-on tooyour Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="4b7c8-165">Kattintson a **IRÁNYÍTÓPULT**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-165">Click **DASHBOARD**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="4b7c8-167">Kattintson a **beállítások**, majd végezze el a következő lépéseket a hello hello **vállalati** lapon:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-167">Click **Settings**, and then perform hello following steps on hello **Company** tab:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="4b7c8-169">a.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-169">a.</span></span> <span data-ttu-id="4b7c8-170">Válassza ki **SAML 2.0 hitelesítéshez használandó**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="4b7c8-171">b.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-171">b.</span></span> <span data-ttu-id="4b7c8-172">Kattintson a **SAML konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="4b7c8-173">A hello **SAML-alapú konfigurációs** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-173">On hello **SAML Configuration** dialog, perform hello following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="4b7c8-175">a.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-175">a.</span></span> <span data-ttu-id="4b7c8-176">A hello **tartomány** szövegmező, írja be a regisztrált tartománya.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-176">In hello **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="4b7c8-177">Ha még nem rendelkezik egy regisztrált tartományt, mégis, lépjen kapcsolatba a laposabb fájlok támogatja-e a csapat keresztül [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="4b7c8-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="4b7c8-178">b.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-178">b.</span></span> <span data-ttu-id="4b7c8-179">A **identitási szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** másolta, amely Azure-portálon alkotnak.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-179">In **Identity Provider URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="4b7c8-180">c.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-180">c.</span></span>  <span data-ttu-id="4b7c8-181">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **szolgáltató Identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-181">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="4b7c8-182">d.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-182">d.</span></span> <span data-ttu-id="4b7c8-183">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="4b7c8-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4b7c8-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4b7c8-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4b7c8-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4b7c8-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4b7c8-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="4b7c8-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4b7c8-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b7c8-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4b7c8-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4b7c8-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4b7c8-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4b7c8-199">a.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-199">a.</span></span> <span data-ttu-id="4b7c8-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4b7c8-201">b.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-201">b.</span></span> <span data-ttu-id="4b7c8-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4b7c8-203">c.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-203">c.</span></span> <span data-ttu-id="4b7c8-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4b7c8-205">d.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-205">d.</span></span> <span data-ttu-id="4b7c8-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="4b7c8-207">Laposabb fájlok tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b7c8-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="4b7c8-208">hello ebben a szakaszban célja toocreate laposabb fájlok Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-208">hello objective of this section is toocreate a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="4b7c8-209">**toocreate Britta Simon meghívta laposabb fájlok, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-209">**toocreate a user called Britta Simon in Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b7c8-210">Jelentkezzen be tooyour **laposabb fájlok** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-210">Sign on tooyour **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="4b7c8-211">Hello bal oldali hello navigációs ablaktábláján kattintson **beállítások**, majd kattintson a hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-211">In hello navigation pane on hello left, click **Settings**, and then click hello **Users** tab.</span></span>
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="4b7c8-213">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="4b7c8-214">A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4b7c8-214">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="4b7c8-216">a.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-216">a.</span></span> <span data-ttu-id="4b7c8-217">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-217">In hello **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="4b7c8-218">b.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-218">b.</span></span> <span data-ttu-id="4b7c8-219">A hello **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-219">In hello **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="4b7c8-220">c.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-220">c.</span></span> <span data-ttu-id="4b7c8-221">A hello **E-mail cím** szövegmező, írja be a Britta tartozó e-mail cím hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-221">In hello **Email Address** textbox, type Britta's email address in hello Azure portal.</span></span>
   
    <span data-ttu-id="4b7c8-222">d.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-222">d.</span></span> <span data-ttu-id="4b7c8-223">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-223">Click **Submit**.</span></span>   


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4b7c8-224">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4b7c8-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4b7c8-225">Ebben a szakaszban engedélyezze Britta Simon megadásával Azure egyszeri bejelentkezés toouse tooFlatter fájlok elérése.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFlatter Files.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4b7c8-227">**tooassign Britta Simon tooFlatter fájlok, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4b7c8-227">**tooassign Britta Simon tooFlatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b7c8-228">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4b7c8-230">Hello alkalmazások listában válassza ki a **laposabb fájlok**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-230">In hello applications list, select **Flatter Files**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="4b7c8-232">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4b7c8-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-234">Click **Add** button.</span></span> <span data-ttu-id="4b7c8-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4b7c8-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4b7c8-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4b7c8-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4b7c8-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4b7c8-240">Testing single sign-on</span></span>

<span data-ttu-id="4b7c8-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4b7c8-242">Hello laposabb fájlok hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour laposabb fájlok alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b7c8-242">When you click hello Flatter Files tile in hello Access Panel, you should get automatically signed-on tooyour Flatter Files application.</span></span>
<span data-ttu-id="4b7c8-243">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4b7c8-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b7c8-244">További források</span><span class="sxs-lookup"><span data-stu-id="4b7c8-244">Additional resources</span></span>

* [<span data-ttu-id="4b7c8-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4b7c8-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4b7c8-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4b7c8-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

