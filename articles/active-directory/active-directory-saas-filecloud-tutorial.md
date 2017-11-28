---
title: "Oktatóanyag: Azure Active Directoryval integrált FileCloud |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FileCloud között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: fe58d01f02d6ce99ee9e2f83e7dc72c4434e13b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="e227a-103">Oktatóanyag: Azure Active Directoryval integrált FileCloud</span><span class="sxs-lookup"><span data-stu-id="e227a-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="e227a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FileCloud az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e227a-104">In this tutorial, you learn how toointegrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e227a-105">FileCloud integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e227a-105">Integrating FileCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e227a-106">Az Azure AD hozzáférési tooFileCloud rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e227a-106">You can control in Azure AD who has access tooFileCloud.</span></span>
- <span data-ttu-id="e227a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFileCloud (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="e227a-107">You can enable your users tooautomatically get signed-on tooFileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e227a-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e227a-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e227a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e227a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e227a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e227a-110">Prerequisites</span></span>

<span data-ttu-id="e227a-111">az Azure AD integrálása FileCloud tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e227a-111">tooconfigure Azure AD integration with FileCloud, you need hello following items:</span></span>

- <span data-ttu-id="e227a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e227a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e227a-113">Egy FileCloud egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e227a-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e227a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e227a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e227a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e227a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e227a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e227a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e227a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e227a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e227a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e227a-118">Scenario description</span></span>
<span data-ttu-id="e227a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e227a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e227a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e227a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e227a-121">Hello gyűjteményből FileCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e227a-121">Adding FileCloud from hello gallery</span></span>
2. <span data-ttu-id="e227a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e227a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-hello-gallery"></a><span data-ttu-id="e227a-123">Hello gyűjteményből FileCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e227a-123">Adding FileCloud from hello gallery</span></span>
<span data-ttu-id="e227a-124">tooconfigure hello integrációja FileCloud az Azure AD-be, meg kell tooadd FileCloud hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e227a-124">tooconfigure hello integration of FileCloud into Azure AD, you need tooadd FileCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e227a-125">**tooadd FileCloud hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e227a-125">**tooadd FileCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e227a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e227a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="e227a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e227a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e227a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e227a-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e227a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e227a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="e227a-133">Hello keresési mezőbe, írja be a **FileCloud**, jelölje be **FileCloud** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-133">In hello search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában FileCloud](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e227a-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e227a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e227a-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FileCloud.</span><span class="sxs-lookup"><span data-stu-id="e227a-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e227a-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FileCloud tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e227a-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FileCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="e227a-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FileCloud közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e227a-138">In other words, a link relationship between an Azure AD user and hello related user in FileCloud needs toobe established.</span></span>

<span data-ttu-id="e227a-139">FileCloud, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e227a-139">In FileCloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e227a-140">tooconfigure és az Azure AD az egyszeri bejelentkezés FileCloud-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e227a-140">tooconfigure and test Azure AD single sign-on with FileCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e227a-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e227a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e227a-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e227a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e227a-143">**[FileCloud tesztfelhasználó létrehozása](#create-a-filecloud-test-user)**  -toohave egy megfelelője a Britta Simon a FileCloud, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e227a-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - toohave a counterpart of Britta Simon in FileCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e227a-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e227a-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e227a-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e227a-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e227a-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e227a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e227a-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az FileCloud alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e227a-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="e227a-148">**az Azure AD tooconfigure egyszeri bejelentkezést a FileCloud, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e227a-148">**tooconfigure Azure AD single sign-on with FileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="e227a-149">Az Azure portál, a hello hello **FileCloud** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e227a-149">In hello Azure portal, on hello **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="e227a-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e227a-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="e227a-153">A hello **FileCloud tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e227a-153">On hello **FileCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk FileCloud tartomány és az URL-címek](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="e227a-155">a.</span><span class="sxs-lookup"><span data-stu-id="e227a-155">a.</span></span> <span data-ttu-id="e227a-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="e227a-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="e227a-157">b.</span><span class="sxs-lookup"><span data-stu-id="e227a-157">b.</span></span> <span data-ttu-id="e227a-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="e227a-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e227a-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e227a-159">These values are not real.</span></span> <span data-ttu-id="e227a-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="e227a-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e227a-161">Ügyfél [FileCloud ügyfél-támogatási csoport](mailto:support@codelathe.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e227a-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) tooget these values.</span></span>

4. <span data-ttu-id="e227a-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e227a-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="e227a-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e227a-166">A hello **FileCloud konfigurációs** kattintson **konfigurálása FileCloud** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e227a-166">On hello **FileCloud Configuration** section, click **Configure FileCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e227a-167">Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e227a-167">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![FileCloud konfiguráció](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="e227a-169">Egy másik webes böngészőablakban, bejelentkezés tooyour FileCloud Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="e227a-169">In a different web browser window, sign-on tooyour FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="e227a-170">A hello bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e227a-170">On hello left navigation pane, click **Settings**.</span></span> 
   
    ![Beállítások szakaszban az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="e227a-172">Kattintson a **SSO** beállításait szakaszon fülre.</span><span class="sxs-lookup"><span data-stu-id="e227a-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Egyszeri bejelentkezés lapon az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="e227a-174">Válassza ki **SAML** , **alapértelmezett egyszeri bejelentkezés típusa** a **egyszeri bejelentkezés (SSO) beállítások** panel.</span><span class="sxs-lookup"><span data-stu-id="e227a-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Egyszeri bejelentkezés beállítások Panel az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="e227a-176">Beillesztés **SAML Entitásazonosító**, amely Azure-portálon való hello másolta **IdP végpont URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e227a-176">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP End Point URL** textbox.</span></span>

    ![IDP End pont URL-cím beviteli mező](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="e227a-178">Nyissa meg a letöltött metaadatfájl a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **IdP metaadatai** a szövegmező **SAML beállítások** panel.</span><span class="sxs-lookup"><span data-stu-id="e227a-178">Open your downloaded metadata file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![Alkalmazás ügyféloldali IDP Meta adatszakasz](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="e227a-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="e227a-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e227a-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e227a-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e227a-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e227a-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e227a-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e227a-184">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e227a-184">Create an Azure AD test user</span></span>

<span data-ttu-id="e227a-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e227a-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e227a-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e227a-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e227a-188">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-188">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e227a-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e227a-190">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e227a-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e227a-192">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e227a-194">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e227a-194">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e227a-196">a.</span><span class="sxs-lookup"><span data-stu-id="e227a-196">a.</span></span> <span data-ttu-id="e227a-197">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e227a-197">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e227a-198">b.</span><span class="sxs-lookup"><span data-stu-id="e227a-198">b.</span></span> <span data-ttu-id="e227a-199">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="e227a-199">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e227a-200">c.</span><span class="sxs-lookup"><span data-stu-id="e227a-200">c.</span></span> <span data-ttu-id="e227a-201">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e227a-201">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e227a-202">d.</span><span class="sxs-lookup"><span data-stu-id="e227a-202">d.</span></span> <span data-ttu-id="e227a-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="e227a-204">FileCloud tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e227a-204">Create a FileCloud test user</span></span>

<span data-ttu-id="e227a-205">hello ebben a szakaszban célja toocreate FileCloud Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e227a-205">hello objective of this section is toocreate a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="e227a-206">FileCloud támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="e227a-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="e227a-207">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="e227a-207">There is no action item for you in this section.</span></span> <span data-ttu-id="e227a-208">Új felhasználó jön létre egy kísérlet tooaccess FileCloud során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e227a-208">A new user is created during an attempt tooaccess FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="e227a-209">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [FileCloud ügyfél-támogatási csoport](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="e227a-209">If you need toocreate a user manually, you need toocontact hello [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e227a-210">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e227a-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e227a-211">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFileCloud megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e227a-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFileCloud.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="e227a-213">**tooassign Britta Simon tooFileCloud, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e227a-213">**tooassign Britta Simon tooFileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="e227a-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e227a-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e227a-216">Hello alkalmazások listában válassza ki a **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="e227a-216">In hello applications list, select **FileCloud**.</span></span>

    ![hello FileCloud hivatkozásra hello alkalmazások listája](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="e227a-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e227a-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="e227a-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e227a-220">Click **Add** button.</span></span> <span data-ttu-id="e227a-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e227a-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e227a-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e227a-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e227a-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e227a-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e227a-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e227a-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e227a-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e227a-226">Test single sign-on</span></span>

<span data-ttu-id="e227a-227">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="e227a-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="e227a-228">Ha a hozzáférési Panel hello hello FileCloud csempe gombra kattint, automatikusan bejelentkezett tooyour FileCloud alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e227a-228">When you click hello FileCloud tile in hello Access Panel, you should get automatically signed-on tooyour FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e227a-229">További források</span><span class="sxs-lookup"><span data-stu-id="e227a-229">Additional resources</span></span>

* [<span data-ttu-id="e227a-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e227a-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e227a-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e227a-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

