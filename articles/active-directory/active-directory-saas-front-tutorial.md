---
title: "Oktatóanyag: Azure Active Directoryval integrált első |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az első között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 4be363a3d338ec9268f3324daab4a80346ec3131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="bc8b3-103">Oktatóanyag: Azure Active Directoryval integrált első</span><span class="sxs-lookup"><span data-stu-id="bc8b3-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="bc8b3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate előre hozása az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bc8b3-104">In this tutorial, you learn how toointegrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bc8b3-105">Első integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-105">Integrating Front with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bc8b3-106">Az Azure AD hozzáférési tooFront rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-106">You can control in Azure AD who has access tooFront.</span></span>
- <span data-ttu-id="bc8b3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFront (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-107">You can enable your users tooautomatically get signed-on tooFront (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="bc8b3-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="bc8b3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bc8b3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc8b3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bc8b3-110">Prerequisites</span></span>

<span data-ttu-id="bc8b3-111">Első tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-111">tooconfigure Azure AD integration with Front, you need hello following items:</span></span>

- <span data-ttu-id="bc8b3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bc8b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bc8b3-113">Egy első egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bc8b3-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bc8b3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bc8b3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bc8b3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bc8b3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc8b3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bc8b3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bc8b3-118">Scenario description</span></span>
<span data-ttu-id="bc8b3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bc8b3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bc8b3-121">Első hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="bc8b3-121">Adding Front from hello gallery</span></span>
2. <span data-ttu-id="bc8b3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bc8b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-hello-gallery"></a><span data-ttu-id="bc8b3-123">Első hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="bc8b3-123">Adding Front from hello gallery</span></span>
<span data-ttu-id="bc8b3-124">tooconfigure hello integrációs első, az Azure AD-be, meg kell tooadd első hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-124">tooconfigure hello integration of Front into Azure AD, you need tooadd Front from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bc8b3-125">**tooadd első hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bc8b3-125">**tooadd Front from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc8b3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="bc8b3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bc8b3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="bc8b3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="bc8b3-133">Hello keresési mezőbe, írja be a **első**, jelölje be **első** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-133">In hello search box, type **Front**, select **Front** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Első hello eredmények listájában](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bc8b3-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc8b3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="bc8b3-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés első "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bc8b3-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói elöl tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Front is tooa user in Azure AD.</span></span> <span data-ttu-id="bc8b3-138">Ez azt jelenti hello elöl kapcsolódó felhasználói és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-138">In other words, a link relationship between an Azure AD user and hello related user in Front needs toobe established.</span></span>

<span data-ttu-id="bc8b3-139">Az előtérben, rendelje az hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-139">In Front, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bc8b3-140">tooconfigure és az első Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-140">tooconfigure and test Azure AD single sign-on with Front, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bc8b3-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bc8b3-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bc8b3-143">**[Első tesztfelhasználó létrehozása](#create-a-front-test-user)**  -toohave egy megfelelője a Britta Simon elöl csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-143">**[Create a Front test user](#create-a-front-test-user)** - toohave a counterpart of Britta Simon in Front that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bc8b3-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bc8b3-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bc8b3-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc8b3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bc8b3-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az első alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="bc8b3-148">**az első, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bc8b3-148">**tooconfigure Azure AD single sign-on with Front, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc8b3-149">Az Azure portál, a hello hello **első** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-149">In hello Azure portal, on hello **Front** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="bc8b3-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="bc8b3-153">A hello **első tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-153">On hello **Front Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="bc8b3-155">a.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-155">a.</span></span> <span data-ttu-id="bc8b3-156">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="bc8b3-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="bc8b3-157">b.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-157">b.</span></span> <span data-ttu-id="bc8b3-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="bc8b3-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="bc8b3-159">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-159">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="bc8b3-161">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="bc8b3-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="bc8b3-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-162">These values are not real.</span></span> <span data-ttu-id="bc8b3-163">Ezeket az értékeket a hello tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím amelyeket később az oktatóanyag, vagy forduljon a frissítés [első ügyfél-támogatási csoport](mailto:support@frontapp.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) tooget these values.</span></span> 

5. <span data-ttu-id="bc8b3-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="bc8b3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="bc8b3-168">A hello **első konfigurációs** kattintson **első konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-168">On hello **Front Configuration** section, click **Configure Front** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bc8b3-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bc8b3-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="bc8b3-171">Bejelentkezés tooyour első Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-171">Sign-on tooyour Front tenant as an administrator.</span></span>

9. <span data-ttu-id="bc8b3-172">Nyissa meg túl**beállítások (fogaskerék ikonjára ikon hello bal oldalsávon hello alján) > Beállítások**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-172">Go too**Settings (cog icon at hello bottom of hello left sidebar) > Preferences**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="bc8b3-174">Kattintson a **egyszeri bejelentkezés** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-174">Click **Single Sign On** link.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="bc8b3-176">Válassza ki **SAML** hello legördülő listájában **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-176">Select **SAML** in hello drop-down list of **Single Sign On**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="bc8b3-178">A hello **belépési pont** szövegmezőbe írja be a hello értéket **egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-178">In hello **Entry Point** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="bc8b3-180">Nyissa meg a letöltött **Certificate(Base64)** fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **tanúsítvány aláírása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-180">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Signing certificate** textbox.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="bc8b3-182">A hello **szolgáltató Szolgáltatásbeállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-182">On hello **Service provider settings** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="bc8b3-184">a.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-184">a.</span></span> <span data-ttu-id="bc8b3-185">Hello értékének másolása **Entitásazonosító** és illessze be hello **azonosító** textbox **első tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-185">Copy hello value of **Entity ID** and paste it into hello **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="bc8b3-186">b.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-186">b.</span></span> <span data-ttu-id="bc8b3-187">Hello értékének másolása **ACS URL-cím** és illessze be hello **bejelentkezési URL-cím** textbox **első tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-187">Copy hello value of **ACS URL** and paste it into hello **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="bc8b3-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="bc8b3-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bc8b3-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bc8b3-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bc8b3-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bc8b3-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bc8b3-192">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="bc8b3-192">Create an Azure AD test user</span></span>

<span data-ttu-id="bc8b3-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="bc8b3-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bc8b3-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc8b3-196">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="bc8b3-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="bc8b3-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="bc8b3-202">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc8b3-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="bc8b3-204">a.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-204">a.</span></span> <span data-ttu-id="bc8b3-205">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bc8b3-206">b.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-206">b.</span></span> <span data-ttu-id="bc8b3-207">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="bc8b3-208">c.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-208">c.</span></span> <span data-ttu-id="bc8b3-209">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="bc8b3-210">d.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-210">d.</span></span> <span data-ttu-id="bc8b3-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="bc8b3-212">Első tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc8b3-212">Create a Front test user</span></span>

<span data-ttu-id="bc8b3-213">Ebben a szakaszban egy Britta Simon elöl nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="bc8b3-214">Együttműködve [első ügyfél-támogatási csoport](mailto:support@frontapp.com) felhasználót is hozzáadhat hello hello első platform.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-214">Work with [Front Client support team](mailto:support@frontapp.com) to add hello users in hello Front platform.</span></span> <span data-ttu-id="bc8b3-215">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="bc8b3-216">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="bc8b3-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="bc8b3-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFront megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFront.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="bc8b3-219">**tooassign Britta Simon tooFront, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bc8b3-219">**tooassign Britta Simon tooFront, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc8b3-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bc8b3-222">Hello alkalmazások listában válassza ki a **első**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-222">In hello applications list, select **Front**.</span></span>

    ![hello első hivatkozásra hello alkalmazások listája](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="bc8b3-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="bc8b3-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-226">Click **Add** button.</span></span> <span data-ttu-id="bc8b3-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="bc8b3-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bc8b3-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bc8b3-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bc8b3-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bc8b3-232">Test single sign-on</span></span>

<span data-ttu-id="bc8b3-233">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSOconfiguration használatával hello.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-233">hello objective of this section is tootest your Azure AD SSOconfiguration using hello Access Panel.</span></span>

<span data-ttu-id="bc8b3-234">Hello első csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour első alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="bc8b3-234">When you click hello Front tile in hello Access Panel, you should get automatically signed-on tooyour Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bc8b3-235">További források</span><span class="sxs-lookup"><span data-stu-id="bc8b3-235">Additional resources</span></span>

* [<span data-ttu-id="bc8b3-236">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bc8b3-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bc8b3-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bc8b3-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

