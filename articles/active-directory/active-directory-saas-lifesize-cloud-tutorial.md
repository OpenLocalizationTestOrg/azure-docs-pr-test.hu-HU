---
title: "Oktatóanyag: Azure Active Directory-integráció a Lifesize felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Lifesize felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="d860d-103">Oktatóanyag: Azure Active Directoryval integrált Lifesize felhő</span><span class="sxs-lookup"><span data-stu-id="d860d-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="d860d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lifesize felhőalapú Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d860d-104">In this tutorial, you learn how toointegrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d860d-105">Lifesize felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d860d-105">Integrating Lifesize Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d860d-106">Megadhatja a hozzáférés tooLifesize felhő rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d860d-106">You can control in Azure AD who has access tooLifesize Cloud</span></span>
- <span data-ttu-id="d860d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLifesize felhő (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d860d-107">You can enable your users tooautomatically get signed-on tooLifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d860d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d860d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d860d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d860d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d860d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d860d-110">Prerequisites</span></span>

<span data-ttu-id="d860d-111">tooconfigure Lifesize felhőalapú Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="d860d-111">tooconfigure Azure AD integration with Lifesize Cloud, you need hello following items:</span></span>

- <span data-ttu-id="d860d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d860d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d860d-113">Egy Lifesize felhő egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d860d-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d860d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d860d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d860d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d860d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d860d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d860d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d860d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d860d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d860d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d860d-118">Scenario description</span></span>
<span data-ttu-id="d860d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d860d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d860d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d860d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d860d-121">Hello gyűjteményből Lifesize felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d860d-121">Adding Lifesize Cloud from hello gallery</span></span>
2. <span data-ttu-id="d860d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d860d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-hello-gallery"></a><span data-ttu-id="d860d-123">Hello gyűjteményből Lifesize felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d860d-123">Adding Lifesize Cloud from hello gallery</span></span>
<span data-ttu-id="d860d-124">tooconfigure hello integrációs Lifesize felhőalapú, az Azure AD-be, meg kell tooadd Lifesize felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d860d-124">tooconfigure hello integration of Lifesize Cloud into Azure AD, you need tooadd Lifesize Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d860d-125">**tooadd Lifesize felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d860d-125">**tooadd Lifesize Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d860d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d860d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d860d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d860d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d860d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d860d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d860d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d860d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d860d-133">Hello keresési mezőbe, írja be a **Lifesize felhő**.</span><span class="sxs-lookup"><span data-stu-id="d860d-133">In hello search box, type **Lifesize Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="d860d-135">A hello eredmények panelen válassza ki a **Lifesize felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-135">In hello results panel, select **Lifesize Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d860d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d860d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d860d-138">Ebben a szakaszban konfigurálhatja, és a Lifesize felhőalapú Azure AD az egyszeri bejelentkezés teszt "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="d860d-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d860d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Lifesize felhőben tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d860d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lifesize Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="d860d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Lifesize felhőben közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="d860d-140">In other words, a link relationship between an Azure AD user and hello related user in Lifesize Cloud needs toobe established.</span></span>

<span data-ttu-id="d860d-141">Lifesize felhő hozzárendelése hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d860d-141">In Lifesize Cloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d860d-142">tooconfigure és a Lifesize felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d860d-142">tooconfigure and test Azure AD single sign-on with Lifesize Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d860d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d860d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d860d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d860d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d860d-145">**[Lifesize felhő tesztfelhasználó létrehozása](#creating-a-lifesize-cloud-test-user)**  -toohave egy megfelelője a Britta Simon Lifesize felhőben található, a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d860d-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - toohave a counterpart of Britta Simon in Lifesize Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d860d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d860d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d860d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d860d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d860d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d860d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d860d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lifesize felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d860d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="d860d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Lifesize felhőalapú, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d860d-150">**tooconfigure Azure AD single sign-on with Lifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="d860d-151">Az Azure portál, a hello hello **Lifesize felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d860d-151">In hello Azure portal, on hello **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d860d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d860d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="d860d-155">A hello **Lifesize felhőalapú tartományt és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d860d-155">On hello **Lifesize Cloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="d860d-157">a.</span><span class="sxs-lookup"><span data-stu-id="d860d-157">a.</span></span> <span data-ttu-id="d860d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="d860d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="d860d-159">b.</span><span class="sxs-lookup"><span data-stu-id="d860d-159">b.</span></span> <span data-ttu-id="d860d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="d860d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="d860d-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**, hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="d860d-161">Check **Show advanced URL settings**, perform hello following step:</span></span>  
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="d860d-163">A hello **állapot továbbítása** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="d860d-163">In hello **Relay state** textbox, type a URL using hello following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="d860d-164">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="d860d-164">Please note that these are not hello real values.</span></span> <span data-ttu-id="d860d-165">Ezeket az értékeket a hello rendelkezik tooupdate tényleges bejelentkezési URL-cím, továbbító állapotát és azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d860d-165">you have tooupdate these values with hello actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="d860d-166">Ügyfél [Lifesize felhőalapú ügyfél-támogatási csoport](https://www.lifesize.com/support) tooget bejelentkezési URL-címet, és azonosítóértékek, és lekérheti továbbítási állapotérték SSO konfigurációs kifejtett hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d860d-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) tooget Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in hello tutorial.</span></span>

4. <span data-ttu-id="d860d-167">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d860d-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="d860d-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d860d-171">A hello **Lifesize Felhőkonfiguráció** kattintson **Lifesize felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d860d-171">On hello **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d860d-172">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d860d-172">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="d860d-174">tooget SSO az alkalmazáshoz, a bejelentkezés hello Lifesize felhőalapú alkalmazásnál rendszergazda jogosultságokkal a beállított.</span><span class="sxs-lookup"><span data-stu-id="d860d-174">tooget SSO configured for your application, login into hello Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="d860d-175">Hello jobb felső sarokban lévő kattintson a nevére, és kattintson a hello **speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d860d-175">In hello top right corner click on your name and then click on hello **Advance Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="d860d-177">A Speciális beállítások most kattintson a hello hello **SSO konfigurációs** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d860d-177">In hello Advance Settings now click on hello **SSO Configuration** link.</span></span> <span data-ttu-id="d860d-178">Az nyílik hello egyszeri bejelentkezés konfigurálása lapon a példány.</span><span class="sxs-lookup"><span data-stu-id="d860d-178">It will open hello SSO Configuration page for your instance.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="d860d-180">A következő értékek hello SSO konfigurációs UI hello konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="d860d-180">Now configure hello following values in hello SSO configuration UI.</span></span>    
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="d860d-182">a.</span><span class="sxs-lookup"><span data-stu-id="d860d-182">a.</span></span> <span data-ttu-id="d860d-183">A **Identity Provider kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d860d-183">In **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d860d-184">b.</span><span class="sxs-lookup"><span data-stu-id="d860d-184">b.</span></span>  <span data-ttu-id="d860d-185">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d860d-185">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d860d-186">c.</span><span class="sxs-lookup"><span data-stu-id="d860d-186">c.</span></span> <span data-ttu-id="d860d-187">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d860d-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="d860d-188">d.</span><span class="sxs-lookup"><span data-stu-id="d860d-188">d.</span></span> <span data-ttu-id="d860d-189">Hello SAML attribútum azon hello Keresztnév szövegmezőben adja meg hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="d860d-189">In hello SAML Attribute mappings for hello First Name text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="d860d-190">e.</span><span class="sxs-lookup"><span data-stu-id="d860d-190">e.</span></span> <span data-ttu-id="d860d-191">A SAML attribútum leképezést hello hello **Vezetéknév** szövegmezőbe írja be a hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="d860d-191">In hello SAML Attribute mapping for hello **Last Name** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="d860d-192">f.</span><span class="sxs-lookup"><span data-stu-id="d860d-192">f.</span></span> <span data-ttu-id="d860d-193">A SAML attribútum leképezést hello hello **E-mail** szövegmezőbe írja be a hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="d860d-193">In hello SAML Attribute mapping for hello **Email** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="d860d-194">rákattinthat a hello toocheck hello konfigurációs **teszt** gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-194">toocheck hello configuration you can click on hello **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d860d-195">Sikeres tesztelési toocomplete hello konfigurációs varázsló kell az Azure ad-ben, és is megadhatja a hozzáférés toousers vagy csoportokat, amelyekre hello tesztet hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="d860d-195">For successful testing you need toocomplete hello configuration wizard in Azure AD and also provide access toousers or groups who can perform hello test.</span></span>

12. <span data-ttu-id="d860d-196">Engedélyezze a hello egyszeri bejelentkezés ellenőrzése a következőn: hello **SSO engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-196">Enable hello SSO by checking on hello **Enable SSO** button.</span></span>

13. <span data-ttu-id="d860d-197">Most kattintson a hello **frissítés** gombra, hogy az összes hello-beállítások mentése.</span><span class="sxs-lookup"><span data-stu-id="d860d-197">Now click on hello **Update** button so that all hello settings are saved.</span></span> <span data-ttu-id="d860d-198">Ez a megoldás hello RelayState érték.</span><span class="sxs-lookup"><span data-stu-id="d860d-198">This will generate hello RelayState value.</span></span> <span data-ttu-id="d860d-199">Másolás hello RelayState érték, amely hello szövegmezőben jön létre, illessze be hello **továbbítási állapotot** szövegmező alatt **Lifesize felhőalapú tartományt és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d860d-199">Copy hello RelayState value, which is generated in hello text box, paste it in hello **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="d860d-200">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d860d-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d860d-201">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d860d-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d860d-202">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d860d-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d860d-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d860d-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="d860d-204">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d860d-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d860d-206">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d860d-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d860d-207">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d860d-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d860d-209">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d860d-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d860d-211">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d860d-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d860d-213">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d860d-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d860d-215">a.</span><span class="sxs-lookup"><span data-stu-id="d860d-215">a.</span></span> <span data-ttu-id="d860d-216">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d860d-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d860d-217">b.</span><span class="sxs-lookup"><span data-stu-id="d860d-217">b.</span></span> <span data-ttu-id="d860d-218">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d860d-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d860d-219">c.</span><span class="sxs-lookup"><span data-stu-id="d860d-219">c.</span></span> <span data-ttu-id="d860d-220">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d860d-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d860d-221">d.</span><span class="sxs-lookup"><span data-stu-id="d860d-221">d.</span></span> <span data-ttu-id="d860d-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="d860d-223">Lifesize felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d860d-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="d860d-224">Ebben a szakaszban egy felhasználó Britta Simon nevű Lifesize felhőben hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d860d-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="d860d-225">Lifesize felhő támogatja, a felhasználók automatikus átadása.</span><span class="sxs-lookup"><span data-stu-id="d860d-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="d860d-226">Az Azure AD a sikeres hitelesítést követően hello felhasználói automatikusan megkapják a hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d860d-226">After successful authentication at Azure AD, hello user will be automatically provisioned in hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d860d-227">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d860d-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d860d-228">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooLifesize felhő megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="d860d-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLifesize Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d860d-230">**tooassign Britta Simon tooLifesize felhő, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d860d-230">**tooassign Britta Simon tooLifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="d860d-231">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d860d-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d860d-233">Hello alkalmazások listában válassza ki a **Lifesize felhő**.</span><span class="sxs-lookup"><span data-stu-id="d860d-233">In hello applications list, select **Lifesize Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="d860d-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d860d-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d860d-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d860d-237">Click **Add** button.</span></span> <span data-ttu-id="d860d-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d860d-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d860d-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d860d-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d860d-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d860d-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d860d-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d860d-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d860d-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d860d-243">Testing single sign-on</span></span>

<span data-ttu-id="d860d-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d860d-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d860d-245">Hello Lifesize felhő hello hozzáférési Panel csempére kattintva Lifesize felhőalapú alkalmazásnál bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d860d-245">When you click hello Lifesize Cloud tile in hello Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="d860d-246">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d860d-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d860d-247">További források</span><span class="sxs-lookup"><span data-stu-id="d860d-247">Additional resources</span></span>

* [<span data-ttu-id="d860d-248">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d860d-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d860d-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d860d-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

