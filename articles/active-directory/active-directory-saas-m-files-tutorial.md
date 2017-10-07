---
title: "Oktatóanyag: Azure Active Directoryval integrált M-fájlok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az M-fájlok között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e1d268da53312b1d2c12e29d66b1a5b66d0cdcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="51986-103">Oktatóanyag: Azure Active Directoryval integrált M-fájlok</span><span class="sxs-lookup"><span data-stu-id="51986-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="51986-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate M-fájlok az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="51986-104">In this tutorial, you learn how toointegrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="51986-105">M-fájlok integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="51986-105">Integrating M-Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="51986-106">Szabályozhatja az Azure AD, aki hozzáféréssel tooM-fájlokat</span><span class="sxs-lookup"><span data-stu-id="51986-106">You can control in Azure AD who has access tooM-Files</span></span>
- <span data-ttu-id="51986-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooM-fájlok (egyszeri bejelentkezés) az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="51986-107">You can enable your users tooautomatically get signed-on tooM-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="51986-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="51986-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="51986-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="51986-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51986-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="51986-110">Prerequisites</span></span>

<span data-ttu-id="51986-111">M-fájlok az Azure AD integrálása tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="51986-111">tooconfigure Azure AD integration with M-Files, you need hello following items:</span></span>

- <span data-ttu-id="51986-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="51986-112">An Azure AD subscription</span></span>
- <span data-ttu-id="51986-113">A M-fájlok az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="51986-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="51986-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="51986-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="51986-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="51986-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="51986-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="51986-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="51986-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51986-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="51986-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="51986-118">Scenario description</span></span>
<span data-ttu-id="51986-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="51986-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="51986-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="51986-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="51986-121">M-fájlok hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="51986-121">Adding M-Files from hello gallery</span></span>
2. <span data-ttu-id="51986-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="51986-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-hello-gallery"></a><span data-ttu-id="51986-123">M-fájlok hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="51986-123">Adding M-Files from hello gallery</span></span>
<span data-ttu-id="51986-124">tooconfigure hello integrációs M-fájlok az Azure AD-be, meg kell tooadd M-fájlok hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="51986-124">tooconfigure hello integration of M-Files into Azure AD, you need tooadd M-Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="51986-125">**M-fájlok tooadd hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="51986-125">**tooadd M-Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="51986-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="51986-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="51986-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="51986-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="51986-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="51986-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="51986-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="51986-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="51986-133">Hello keresési mezőbe, írja be a **M-fájlok**.</span><span class="sxs-lookup"><span data-stu-id="51986-133">In hello search box, type **M-Files**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="51986-135">A hello eredmények panelen válassza ki a **M-fájlok**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="51986-135">In hello results panel, select **M-Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="51986-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="51986-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="51986-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés M-fájlokat a "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="51986-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="51986-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói M-fájlokban szereplő tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="51986-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in M-Files is tooa user in Azure AD.</span></span> <span data-ttu-id="51986-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello M-fájlokban szereplő közötti kapcsolat kapcsolatot kell létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="51986-140">In other words, a link relationship between an Azure AD user and hello related user in M-Files needs toobe established.</span></span>

<span data-ttu-id="51986-141">M-fájlok, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="51986-141">In M-Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="51986-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése M-fájlokkal, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="51986-142">tooconfigure and test Azure AD single sign-on with M-Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="51986-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="51986-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="51986-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="51986-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="51986-145">**[M-fájlok tesztfelhasználó létrehozása](#creating-a-m-files-test-user)**  -toohave egy megfelelője a Britta Simon M-fájlok, amelyek felhasználói csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="51986-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - toohave a counterpart of Britta Simon in M-Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="51986-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="51986-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="51986-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="51986-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="51986-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="51986-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="51986-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az M-fájlok alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="51986-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="51986-150">**az Azure AD tooconfigure egyszeri bejelentkezés M-fájlokkal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="51986-150">**tooconfigure Azure AD single sign-on with M-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="51986-151">Az Azure portál, a hello hello **M-fájlok** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="51986-151">In hello Azure portal, on hello **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="51986-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="51986-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="51986-155">A hello **M-fájlok tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="51986-155">On hello **M-Files Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="51986-157">a.</span><span class="sxs-lookup"><span data-stu-id="51986-157">a.</span></span> <span data-ttu-id="51986-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="51986-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="51986-159">b.</span><span class="sxs-lookup"><span data-stu-id="51986-159">b.</span></span> <span data-ttu-id="51986-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="51986-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="51986-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="51986-161">These values are not real.</span></span> <span data-ttu-id="51986-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="51986-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="51986-163">Ügyfél [M-fájlok ügyfél-támogatási csoport](mailto:support@m-files.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="51986-163">Contact [M-Files Client support team](mailto:support@m-files.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="51986-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51986-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="51986-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="51986-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="51986-168">az alkalmazáshoz konfigurált SSO tooget, forduljon a [M-fájlok támogatási csoport](mailto:support@m-files.com) és azokat hello letöltött metaadatok.</span><span class="sxs-lookup"><span data-stu-id="51986-168">tooget SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them hello downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="51986-169">Hajtsa végre hello lépések, ha azt szeretné, egyszeri Bejelentkezéses tooconfigure meg M-fájl asztali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51986-169">Follow hello next steps if you want tooconfigure SSO for you M-File desktop application.</span></span> <span data-ttu-id="51986-170">Nincsenek további lépésre szükség, ha kizárólag tooconfigure SSO M-fájlokhoz webes verzió.</span><span class="sxs-lookup"><span data-stu-id="51986-170">No extra steps are required if you only want tooconfigure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="51986-171">Hajtsa végre a hello következő lépések tooconfigure hello M-fájl asztali alkalmazás tooenable egyszeri bejelentkezés az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="51986-171">Follow hello next steps tooconfigure hello M-File desktop application tooenable SSO with Azure AD.</span></span> <span data-ttu-id="51986-172">M-fájlok, toodownload lépjen túl[M-fájlok letöltése](https://www.m-files.com/en/download-latest-version) lap.</span><span class="sxs-lookup"><span data-stu-id="51986-172">toodownload M-Files, go too[M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="51986-173">Nyissa meg hello **M-fájlok asztali beállítások** ablak.</span><span class="sxs-lookup"><span data-stu-id="51986-173">Open hello **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="51986-174">Kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="51986-174">Then, click **Add**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="51986-176">A hello **tárolói kapcsolat dokumentumtulajdonságok** ablak, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="51986-176">On hello **Document Vault Connection Properties** window, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="51986-178">A szakasz kiszolgálótípus hello hello értékek az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="51986-178">Under hello Server section type, hello values as follows:</span></span>  

    <span data-ttu-id="51986-179">a.</span><span class="sxs-lookup"><span data-stu-id="51986-179">a.</span></span> <span data-ttu-id="51986-180">A **neve**, típus `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="51986-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="51986-181">b.</span><span class="sxs-lookup"><span data-stu-id="51986-181">b.</span></span> <span data-ttu-id="51986-182">A **portszám**, típus **4466**.</span><span class="sxs-lookup"><span data-stu-id="51986-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="51986-183">c.</span><span class="sxs-lookup"><span data-stu-id="51986-183">c.</span></span> <span data-ttu-id="51986-184">A **protokoll**, jelölje be **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="51986-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="51986-185">d.</span><span class="sxs-lookup"><span data-stu-id="51986-185">d.</span></span> <span data-ttu-id="51986-186">A hello **hitelesítési** mezőben válassza **adott Windows felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="51986-186">In hello **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="51986-187">Ezt követően kéri aláíró lapot.</span><span class="sxs-lookup"><span data-stu-id="51986-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="51986-188">Helyezze be az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="51986-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="51986-189">e.</span><span class="sxs-lookup"><span data-stu-id="51986-189">e.</span></span> <span data-ttu-id="51986-190">A hello **tároló kiszolgálón**, válassza ki a megfelelő tárolót kiszolgálón hello.</span><span class="sxs-lookup"><span data-stu-id="51986-190">For hello **Vault on Server**,  select hello corresponding vault on server.</span></span>
 
    <span data-ttu-id="51986-191">f.</span><span class="sxs-lookup"><span data-stu-id="51986-191">f.</span></span> <span data-ttu-id="51986-192">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51986-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="51986-193">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="51986-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="51986-194">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="51986-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="51986-195">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="51986-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="51986-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="51986-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="51986-197">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="51986-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="51986-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="51986-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="51986-200">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="51986-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="51986-202">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="51986-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="51986-204">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51986-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="51986-206">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="51986-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="51986-208">a.</span><span class="sxs-lookup"><span data-stu-id="51986-208">a.</span></span> <span data-ttu-id="51986-209">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="51986-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="51986-210">b.</span><span class="sxs-lookup"><span data-stu-id="51986-210">b.</span></span> <span data-ttu-id="51986-211">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="51986-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="51986-212">c.</span><span class="sxs-lookup"><span data-stu-id="51986-212">c.</span></span> <span data-ttu-id="51986-213">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="51986-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="51986-214">d.</span><span class="sxs-lookup"><span data-stu-id="51986-214">d.</span></span> <span data-ttu-id="51986-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="51986-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="51986-216">M-fájlok tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="51986-216">Creating a M-Files test user</span></span>

<span data-ttu-id="51986-217">hello ebben a szakaszban célja toocreate M-fájlok Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="51986-217">hello objective of this section is toocreate a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="51986-218">Együttműködve [M-fájlok támogatási csoport](mailto:support@m-files.com) tooadd hello felhasználók hello M-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="51986-218">Work with  [M-Files support team](mailto:support@m-files.com) tooadd hello users in hello M-Files.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="51986-219">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51986-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="51986-220">Ebben a szakaszban engedélyezése Britta Simon toouse Azure egyszeri bejelentkezés biztosítása tooM-fájlok elérése.</span><span class="sxs-lookup"><span data-stu-id="51986-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooM-Files.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="51986-222">**tooassign Britta Simon tooM-fájlok, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="51986-222">**tooassign Britta Simon tooM-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="51986-223">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="51986-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="51986-225">Hello alkalmazások listában válassza ki a **M-fájlok**.</span><span class="sxs-lookup"><span data-stu-id="51986-225">In hello applications list, select **M-Files**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="51986-227">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="51986-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="51986-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="51986-229">Click **Add** button.</span></span> <span data-ttu-id="51986-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51986-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="51986-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="51986-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="51986-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51986-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="51986-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51986-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="51986-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="51986-235">Testing single sign-on</span></span>

<span data-ttu-id="51986-236">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="51986-236">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="51986-237">Hello kattintva M-fájlok a hozzáférési Panel hello csempéjén kattintson a jobb automatikusan bejelentkezett tooyour M-fájlok alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="51986-237">When you click hello M-Files tile in hello Access Panel, you should get automatically signed-on tooyour M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51986-238">További források</span><span class="sxs-lookup"><span data-stu-id="51986-238">Additional resources</span></span>

* [<span data-ttu-id="51986-239">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="51986-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="51986-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="51986-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

