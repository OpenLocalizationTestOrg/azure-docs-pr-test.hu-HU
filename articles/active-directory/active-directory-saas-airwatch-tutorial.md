---
title: "Oktatóanyag: Azure Active Directoryval integrált AirWatch |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és AirWatch között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="a91e3-103">Oktatóanyag: Azure Active Directoryval integrált AirWatch</span><span class="sxs-lookup"><span data-stu-id="a91e3-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="a91e3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate AirWatch az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a91e3-104">In this tutorial, you learn how toointegrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a91e3-105">AirWatch integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-105">Integrating AirWatch with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a91e3-106">Megadhatja a hozzáférés tooAirWatch rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a91e3-106">You can control in Azure AD who has access tooAirWatch</span></span>
- <span data-ttu-id="a91e3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAirWatch (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a91e3-107">You can enable your users tooautomatically get signed-on tooAirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a91e3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a91e3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a91e3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a91e3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a91e3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a91e3-110">Prerequisites</span></span>

<span data-ttu-id="a91e3-111">az Azure AD integrálása AirWatch tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-111">tooconfigure Azure AD integration with AirWatch, you need hello following items:</span></span>

- <span data-ttu-id="a91e3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a91e3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a91e3-113">Egy AirWatch egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a91e3-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a91e3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a91e3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a91e3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a91e3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a91e3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a91e3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a91e3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a91e3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a91e3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a91e3-118">Scenario description</span></span>
<span data-ttu-id="a91e3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a91e3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a91e3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a91e3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a91e3-121">Hello gyűjteményből AirWatch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a91e3-121">Adding AirWatch from hello gallery</span></span>
2. <span data-ttu-id="a91e3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a91e3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-hello-gallery"></a><span data-ttu-id="a91e3-123">Hello gyűjteményből AirWatch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a91e3-123">Adding AirWatch from hello gallery</span></span>
<span data-ttu-id="a91e3-124">tooconfigure hello integrációja AirWatch az Azure AD-be, meg kell tooadd AirWatch hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a91e3-124">tooconfigure hello integration of AirWatch into Azure AD, you need tooadd AirWatch from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a91e3-125">**tooadd AirWatch hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a91e3-125">**tooadd AirWatch from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a91e3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a91e3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a91e3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a91e3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a91e3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a91e3-133">Hello keresési mezőbe, írja be a **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-133">In hello search box, type **AirWatch**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="a91e3-135">A hello eredmények panelen válassza ki a **AirWatch**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-135">In hello results panel, select **AirWatch**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a91e3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a91e3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a91e3-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AirWatch</span><span class="sxs-lookup"><span data-stu-id="a91e3-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a91e3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó AirWatch tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a91e3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AirWatch is tooa user in Azure AD.</span></span> <span data-ttu-id="a91e3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello AirWatch közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a91e3-140">In other words, a link relationship between an Azure AD user and hello related user in AirWatch needs toobe established.</span></span>

<span data-ttu-id="a91e3-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a AirWatch.</span><span class="sxs-lookup"><span data-stu-id="a91e3-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in AirWatch.</span></span>

<span data-ttu-id="a91e3-142">tooconfigure és az Azure AD az egyszeri bejelentkezés AirWatch-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a91e3-142">tooconfigure and test Azure AD single sign-on with AirWatch, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a91e3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a91e3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a91e3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a91e3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a91e3-145">**[AirWatch tesztfelhasználó létrehozása](#creating-a-airwatch-test-user)**  -toohave egy megfelelője a Britta Simon a AirWatch, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a91e3-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - toohave a counterpart of Britta Simon in AirWatch that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a91e3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a91e3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a91e3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a91e3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a91e3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a91e3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a91e3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az AirWatch alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a91e3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="a91e3-150">**az Azure AD tooconfigure egyszeri bejelentkezést a AirWatch, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a91e3-150">**tooconfigure Azure AD single sign-on with AirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="a91e3-151">Az Azure portál, a hello hello **AirWatch** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-151">In hello Azure portal, on hello **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a91e3-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a91e3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="a91e3-155">A hello **AirWatch tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-155">On hello **AirWatch Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="a91e3-157">a.</span><span class="sxs-lookup"><span data-stu-id="a91e3-157">a.</span></span> <span data-ttu-id="a91e3-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="a91e3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="a91e3-159">b.</span><span class="sxs-lookup"><span data-stu-id="a91e3-159">b.</span></span> <span data-ttu-id="a91e3-160">A hello **azonosító** szövegmezőhöz hello érték típusa`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="a91e3-160">In hello **Identifier** textbox, type hello value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a91e3-161">Ez az érték nincs valós hello.</span><span class="sxs-lookup"><span data-stu-id="a91e3-161">This value is not hello real.</span></span> <span data-ttu-id="a91e3-162">Ez az érték frissítése hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a91e3-162">Update this value with hello actual Sign-on URL.</span></span> <span data-ttu-id="a91e3-163">Ügyfél [AirWatch ügyfél-támogatási csoport](http://www.air-watch.com/company/contact-us/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="a91e3-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) tooget this value.</span></span> 
 
4. <span data-ttu-id="a91e3-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a91e3-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="a91e3-166">A hello **AirWatch konfigurációs** kattintson **konfigurálása AirWatch** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a91e3-166">On hello **AirWatch Configuration** section, click **Configure AirWatch** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a91e3-167">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a91e3-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="a91e3-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-169">Click **Save** button.</span></span>

    <span data-ttu-id="a91e3-170">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="a91e3-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="a91e3-171">Egy másik webes böngészőablakban jelentkezzen tooyour AirWatch vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a91e3-171">In a different web browser window, log in tooyour AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="a91e3-172">Hello bal oldali navigációs ablaktábláján kattintson **fiókok**, és kattintson a **rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-172">In hello left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="a91e3-173">![A rendszergazdák](./media/active-directory-saas-airwatch-tutorial/ic791920.png "rendszergazdák")</span><span class="sxs-lookup"><span data-stu-id="a91e3-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="a91e3-174">Bontsa ki a hello **beállítások** menüben, majd kattintson **címtárszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-174">Expand hello **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="a91e3-175">![Beállítások](./media/active-directory-saas-airwatch-tutorial/ic791921.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="a91e3-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="a91e3-176">Hello kattintson **felhasználói** lap hello **Base DN** szövegmező, írja be a tartomány nevét, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-176">Click hello **User** tab, in hello **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="a91e3-177">![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791922.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="a91e3-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="a91e3-178">Kattintson a hello **Server** fülre.</span><span class="sxs-lookup"><span data-stu-id="a91e3-178">Click hello **Server** tab.</span></span>
   
   <span data-ttu-id="a91e3-179">![Kiszolgáló](./media/active-directory-saas-airwatch-tutorial/ic791923.png "kiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="a91e3-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="a91e3-180">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-180">Perform hello following steps:</span></span>
    
    <span data-ttu-id="a91e3-181">![Töltse fel](./media/active-directory-saas-airwatch-tutorial/ic791924.png "feltöltése")</span><span class="sxs-lookup"><span data-stu-id="a91e3-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="a91e3-182">a.</span><span class="sxs-lookup"><span data-stu-id="a91e3-182">a.</span></span> <span data-ttu-id="a91e3-183">Mint **Directory típusa**, jelölje be **nincs**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="a91e3-184">b.</span><span class="sxs-lookup"><span data-stu-id="a91e3-184">b.</span></span> <span data-ttu-id="a91e3-185">Válassza ki **SAML-alapú hitelesítéshez használandó**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="a91e3-186">c.</span><span class="sxs-lookup"><span data-stu-id="a91e3-186">c.</span></span> <span data-ttu-id="a91e3-187">tooupload hello a letöltött tanúsítvány, kattintson a **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-187">tooupload hello downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="a91e3-188">A hello **kérelem** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-188">In hello **Request** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="a91e3-189">![Kérelem](./media/active-directory-saas-airwatch-tutorial/ic791925.png "kérése")</span><span class="sxs-lookup"><span data-stu-id="a91e3-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="a91e3-190">a.</span><span class="sxs-lookup"><span data-stu-id="a91e3-190">a.</span></span> <span data-ttu-id="a91e3-191">Mint **kötési típus kérése**, jelölje be **POST**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="a91e3-192">b.</span><span class="sxs-lookup"><span data-stu-id="a91e3-192">b.</span></span> <span data-ttu-id="a91e3-193">Az Azure portál, a hello hello **konfigurálhatja az egyszeri bejelentkezés Airwatch** párbeszédpanel lap, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **identitásszolgáltató Az egyszeri bejelentkezés az URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a91e3-193">In hello Azure portal, on hello **Configure single sign-on at Airwatch** dialog page, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="a91e3-194">c.</span><span class="sxs-lookup"><span data-stu-id="a91e3-194">c.</span></span> <span data-ttu-id="a91e3-195">Mint **NameID formátum**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="a91e3-196">d.</span><span class="sxs-lookup"><span data-stu-id="a91e3-196">d.</span></span> <span data-ttu-id="a91e3-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-197">Click **Save**.</span></span>

14. <span data-ttu-id="a91e3-198">Kattintson a hello **felhasználói** újra fülre.</span><span class="sxs-lookup"><span data-stu-id="a91e3-198">Click hello **User** tab again.</span></span>
    
    <span data-ttu-id="a91e3-199">![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791926.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="a91e3-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="a91e3-200">A hello **attribútum** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-200">In hello **Attribute** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="a91e3-201">![Attribútum](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribútum")</span><span class="sxs-lookup"><span data-stu-id="a91e3-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="a91e3-202">a.</span><span class="sxs-lookup"><span data-stu-id="a91e3-202">a.</span></span> <span data-ttu-id="a91e3-203">A hello **objektumazonosító** szövegmezőhöz típus **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-203">In hello **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="a91e3-204">b.</span><span class="sxs-lookup"><span data-stu-id="a91e3-204">b.</span></span> <span data-ttu-id="a91e3-205">A hello **felhasználónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-205">In hello **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="a91e3-206">c.</span><span class="sxs-lookup"><span data-stu-id="a91e3-206">c.</span></span> <span data-ttu-id="a91e3-207">A hello **megjelenített név** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-207">In hello **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="a91e3-208">d.</span><span class="sxs-lookup"><span data-stu-id="a91e3-208">d.</span></span> <span data-ttu-id="a91e3-209">A hello **Utónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-209">In hello **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="a91e3-210">e.</span><span class="sxs-lookup"><span data-stu-id="a91e3-210">e.</span></span> <span data-ttu-id="a91e3-211">A hello **Vezetéknév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-211">In hello **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="a91e3-212">f.</span><span class="sxs-lookup"><span data-stu-id="a91e3-212">f.</span></span> <span data-ttu-id="a91e3-213">A hello **E-mail** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-213">In hello **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="a91e3-214">g.</span><span class="sxs-lookup"><span data-stu-id="a91e3-214">g.</span></span> <span data-ttu-id="a91e3-215">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a91e3-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a91e3-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="a91e3-217">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a91e3-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a91e3-219">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a91e3-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a91e3-220">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a91e3-222">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a91e3-224">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a91e3-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a91e3-226">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a91e3-228">a.</span><span class="sxs-lookup"><span data-stu-id="a91e3-228">a.</span></span> <span data-ttu-id="a91e3-229">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a91e3-230">b.</span><span class="sxs-lookup"><span data-stu-id="a91e3-230">b.</span></span> <span data-ttu-id="a91e3-231">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a91e3-231">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="a91e3-232">c.</span><span class="sxs-lookup"><span data-stu-id="a91e3-232">c.</span></span> <span data-ttu-id="a91e3-233">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a91e3-234">d.</span><span class="sxs-lookup"><span data-stu-id="a91e3-234">d.</span></span> <span data-ttu-id="a91e3-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="a91e3-236">AirWatch tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a91e3-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="a91e3-237">az Azure AD tooenable felhasználók toolog tooAirWatch, az ezeket ki kell építenie a tooAirWatch.</span><span class="sxs-lookup"><span data-stu-id="a91e3-237">tooenable Azure AD users toolog in tooAirWatch, they must be provisioned in tooAirWatch.</span></span>

* <span data-ttu-id="a91e3-238">AirWatch, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="a91e3-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="a91e3-239">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a91e3-239">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="a91e3-240">Jelentkezzen be tooyour **AirWatch** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a91e3-240">Log in tooyour **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="a91e3-241">A bal oldalon hello hello navigációs ablaktábláján kattintson **fiókok**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-241">In hello navigation pane on hello left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="a91e3-242">![Felhasználók](./media/active-directory-saas-airwatch-tutorial/ic791929.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="a91e3-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="a91e3-243">A hello **felhasználók** menüben kattintson a **listanézet**, és kattintson a **Hozzáadás \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-243">In hello **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="a91e3-244">![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791930.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a91e3-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="a91e3-245">A hello **hozzáadása / szerkesztése felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a91e3-245">On hello **Add / Edit User** dialog, perform hello following steps:</span></span>

   <span data-ttu-id="a91e3-246">![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791931.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a91e3-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="a91e3-247">Típus hello **felhasználónév**, **jelszó**, **jelszó megerősítése**, **Utónév**, **Vezetéknév**,  **E-mail cím** egy érvényes Azure Active Directory-fiókot a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="a91e3-247">Type hello **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="a91e3-248">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="a91e3-249">Bármely más AirWatch felhasználói fiók létrehozása eszközök vagy AirWatch tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="a91e3-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a91e3-250">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a91e3-250">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a91e3-251">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAirWatch megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a91e3-251">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAirWatch.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a91e3-253">**tooassign Britta Simon tooAirWatch, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a91e3-253">**tooassign Britta Simon tooAirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="a91e3-254">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-254">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a91e3-256">Hello alkalmazások listában válassza ki a **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-256">In hello applications list, select **AirWatch**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="a91e3-258">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a91e3-258">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a91e3-260">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a91e3-260">Click **Add** button.</span></span> <span data-ttu-id="a91e3-261">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a91e3-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a91e3-263">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a91e3-263">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a91e3-264">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a91e3-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a91e3-265">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a91e3-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a91e3-266">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a91e3-266">Testing single sign-on</span></span>

<span data-ttu-id="a91e3-267">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a91e3-267">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a91e3-268">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="a91e3-268">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="a91e3-269">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a91e3-269">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a91e3-270">További források</span><span class="sxs-lookup"><span data-stu-id="a91e3-270">Additional resources</span></span>

* [<span data-ttu-id="a91e3-271">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a91e3-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a91e3-272">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a91e3-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

