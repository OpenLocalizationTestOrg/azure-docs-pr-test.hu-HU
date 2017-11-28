---
title: "Oktatóanyag: Azure Active Directoryval integrált Wdesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Wdesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 2c278a5e4f50cc362663efc0f826ff0c0fcf6e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a><span data-ttu-id="55262-103">Oktatóanyag: Azure Active Directoryval integrált Wdesk</span><span class="sxs-lookup"><span data-stu-id="55262-103">Tutorial: Azure Active Directory integration with Wdesk</span></span>

<span data-ttu-id="55262-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Wdesk az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="55262-104">In this tutorial, you learn how toointegrate Wdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="55262-105">Wdesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="55262-105">Integrating Wdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="55262-106">Megadhatja a hozzáférés tooWdesk rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="55262-106">You can control in Azure AD who has access tooWdesk</span></span>
- <span data-ttu-id="55262-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWdesk (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="55262-107">You can enable your users tooautomatically get signed-on tooWdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="55262-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="55262-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="55262-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="55262-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="55262-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="55262-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55262-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55262-111">Prerequisites</span></span>

<span data-ttu-id="55262-112">az Azure AD integrálása Wdesk tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="55262-112">tooconfigure Azure AD integration with Wdesk, you need hello following items:</span></span>

- <span data-ttu-id="55262-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="55262-113">An Azure AD subscription</span></span>
- <span data-ttu-id="55262-114">Egy Wdesk egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="55262-114">A Wdesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="55262-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="55262-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="55262-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="55262-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="55262-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="55262-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="55262-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="55262-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="55262-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="55262-119">Scenario description</span></span>
<span data-ttu-id="55262-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="55262-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="55262-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="55262-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="55262-122">Hello gyűjteményből Wdesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55262-122">Adding Wdesk from hello gallery</span></span>
2. <span data-ttu-id="55262-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55262-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wdesk-from-hello-gallery"></a><span data-ttu-id="55262-124">Hello gyűjteményből Wdesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55262-124">Adding Wdesk from hello gallery</span></span>
<span data-ttu-id="55262-125">tooconfigure hello integrációja Wdesk az Azure AD-be, meg kell tooadd Wdesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="55262-125">tooconfigure hello integration of Wdesk into Azure AD, you need tooadd Wdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="55262-126">**tooadd Wdesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55262-126">**tooadd Wdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="55262-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55262-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="55262-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="55262-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="55262-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55262-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="55262-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="55262-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="55262-134">Hello keresési mezőbe, írja be a **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="55262-134">In hello search box, type **Wdesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. <span data-ttu-id="55262-136">A hello eredmények panelen válassza ki a **Wdesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-136">In hello results panel, select **Wdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="55262-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55262-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="55262-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Wdesk</span><span class="sxs-lookup"><span data-stu-id="55262-139">In this section, you configure and test Azure AD single sign-on with Wdesk based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="55262-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Wdesk tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="55262-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="55262-141">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Wdesk közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="55262-141">In other words, a link relationship between an Azure AD user and hello related user in Wdesk needs toobe established.</span></span>

<span data-ttu-id="55262-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Wdesk.</span><span class="sxs-lookup"><span data-stu-id="55262-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wdesk.</span></span>

<span data-ttu-id="55262-143">tooconfigure és az Azure AD az egyszeri bejelentkezés Wdesk-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="55262-143">tooconfigure and test Azure AD single sign-on with Wdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="55262-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="55262-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="55262-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55262-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="55262-146">**[Wdesk tesztfelhasználó létrehozása](#creating-a-wdesk-test-user)**  -toohave egy megfelelője a Britta Simon a Wdesk, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="55262-146">**[Creating a Wdesk test user](#creating-a-wdesk-test-user)** - toohave a counterpart of Britta Simon in Wdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="55262-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55262-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="55262-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="55262-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="55262-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="55262-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="55262-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Wdesk alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="55262-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wdesk application.</span></span>

<span data-ttu-id="55262-151">**az Azure AD tooconfigure egyszeri bejelentkezést a Wdesk, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55262-151">**tooconfigure Azure AD single sign-on with Wdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="55262-152">Az Azure portál, a hello hello **Wdesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="55262-152">In hello Azure portal, on hello **Wdesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="55262-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55262-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. <span data-ttu-id="55262-156">A hello **Wdesk tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett módban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="55262-156">On hello **Wdesk Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    <span data-ttu-id="55262-158">a.</span><span class="sxs-lookup"><span data-stu-id="55262-158">a.</span></span> <span data-ttu-id="55262-159">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="55262-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span></span>

    <span data-ttu-id="55262-160">b.</span><span class="sxs-lookup"><span data-stu-id="55262-160">b.</span></span> <span data-ttu-id="55262-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="55262-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span></span>

4. <span data-ttu-id="55262-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="55262-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="55262-163">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód, hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="55262-163">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    <span data-ttu-id="55262-165">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="55262-165">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="55262-166">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="55262-166">These values are not real.</span></span> <span data-ttu-id="55262-167">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="55262-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="55262-168">Ezek az értékek első WDesk portálról hello SSO konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="55262-168">You get these values from WDesk portal when you configure hello SSO.</span></span> 
  
5. <span data-ttu-id="55262-169">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="55262-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. <span data-ttu-id="55262-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="55262-173">Egy másik webes böngészőablakban, bejelentkezési tooWdesk egy biztonsági-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55262-173">In a different web browser window, login tooWdesk as a Security Administrator.</span></span>

8. <span data-ttu-id="55262-174">Kattintson a balra hello le, **Admin** , és válassza **Fiókadminisztrátor**:</span><span class="sxs-lookup"><span data-stu-id="55262-174">In hello bottom left, click **Admin** and choose **Account Admin**:</span></span>
 
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. <span data-ttu-id="55262-176">Wdesk rendszergazda, lépjen a túl**biztonsági**, majd **SAML** > **SAML beállítások**:</span><span class="sxs-lookup"><span data-stu-id="55262-176">In Wdesk Admin, navigate too**Security**, then **SAML** > **SAML Settings**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. <span data-ttu-id="55262-178">A **általános beállítások**, ellenőrizze a hello **SAML-alapú egyszeri bejelentkezés engedélyezése**:</span><span class="sxs-lookup"><span data-stu-id="55262-178">Under **General Settings**, check hello **Enable SAML Single Sign On**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. <span data-ttu-id="55262-180">A **szolgáltató részletei**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55262-180">Under **Service Provider Details**, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      <span data-ttu-id="55262-182">a.</span><span class="sxs-lookup"><span data-stu-id="55262-182">a.</span></span> <span data-ttu-id="55262-183">Másolás hello **bejelentkezési URL-cím** és illessze be **bejelentkezési URL-cím** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="55262-183">Copy hello **Login URL** and paste it in **Sign-on Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="55262-184">b.</span><span class="sxs-lookup"><span data-stu-id="55262-184">b.</span></span> <span data-ttu-id="55262-185">Másolás hello **metaadatainak URL-címét** és illessze be **azonosító** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="55262-185">Copy hello **Metadata Url** and paste it in **Identifier** textbox on Azure portal.</span></span>
       
      <span data-ttu-id="55262-186">c.</span><span class="sxs-lookup"><span data-stu-id="55262-186">c.</span></span> <span data-ttu-id="55262-187">Másolás hello **ügyfél URL-címe** és illessze be **válasz URL-címen** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="55262-187">Copy hello **Consumer url** and paste it in **Reply Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="55262-188">d.</span><span class="sxs-lookup"><span data-stu-id="55262-188">d.</span></span> <span data-ttu-id="55262-189">Kattintson a **mentése** az Azure portál toosave hello változásairól.</span><span class="sxs-lookup"><span data-stu-id="55262-189">Click **Save** on Azure portal toosave hello changes.</span></span>      

12. <span data-ttu-id="55262-190">Kattintson a **IdP beállításainak konfigurálása** tooopen **IdP beállításainak szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55262-190">Click **Configure IdP Settings** tooopen **Edit IdP Settings** dialog.</span></span> <span data-ttu-id="55262-191">Kattintson a **Choose File** toolocate hello **Metadata.xml** mentett fájlt, Azure-portálon, majd töltse fel.</span><span class="sxs-lookup"><span data-stu-id="55262-191">Click **Choose File** toolocate hello **Metadata.xml** file you saved from Azure portal, then upload it.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. <span data-ttu-id="55262-193">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="55262-193">Click **Save changes**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> <span data-ttu-id="55262-195">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="55262-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="55262-196">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="55262-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="55262-197">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="55262-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="55262-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55262-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="55262-199">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="55262-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="55262-201">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="55262-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="55262-202">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55262-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="55262-204">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="55262-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="55262-206">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55262-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="55262-208">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55262-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="55262-210">a.</span><span class="sxs-lookup"><span data-stu-id="55262-210">a.</span></span> <span data-ttu-id="55262-211">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="55262-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="55262-212">b.</span><span class="sxs-lookup"><span data-stu-id="55262-212">b.</span></span> <span data-ttu-id="55262-213">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="55262-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="55262-214">c.</span><span class="sxs-lookup"><span data-stu-id="55262-214">c.</span></span> <span data-ttu-id="55262-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="55262-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="55262-216">d.</span><span class="sxs-lookup"><span data-stu-id="55262-216">d.</span></span> <span data-ttu-id="55262-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-217">Click **Create**.</span></span>
 
### <a name="creating-a-wdesk-test-user"></a><span data-ttu-id="55262-218">Wdesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55262-218">Creating a Wdesk test user</span></span>

<span data-ttu-id="55262-219">az Azure AD tooenable felhasználók toolog a tooWdesk, akkor ki kell építenie Wdesk be.</span><span class="sxs-lookup"><span data-stu-id="55262-219">tooenable Azure AD users toolog in tooWdesk, they must be provisioned into Wdesk.</span></span> <span data-ttu-id="55262-220">A Wdesk egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="55262-220">In Wdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="55262-221">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="55262-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="55262-222">Jelentkezzen be tooWdesk egy biztonsági-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55262-222">Log in tooWdesk as a Security Administrator.</span></span>
2. <span data-ttu-id="55262-223">Keresse meg a túl**Admin** > **Fiókadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="55262-223">Navigate too**Admin** > **Account Admin**.</span></span>

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. <span data-ttu-id="55262-225">Kattintson a **tagok** alatt **személyek**.</span><span class="sxs-lookup"><span data-stu-id="55262-225">Click **Members** under **People**.</span></span>

4. <span data-ttu-id="55262-226">Most kattintson **tag hozzáadása** tooopen **tag hozzáadása** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="55262-226">Now click **Add Member** tooopen **Add Member** dialog box.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. <span data-ttu-id="55262-228">A **felhasználói** szöveg mezőbe írja be például a felhasználó felhasználóneve hello  **brittasimon@contoso.com**  kattintson **Folytatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-228">In **User** text box, enter hello username of user like **brittasimon@contoso.com** and click **Continue** button.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  <span data-ttu-id="55262-230">Adja meg hello alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="55262-230">Enter hello details as shown below:</span></span>
  
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    <span data-ttu-id="55262-232">a.</span><span class="sxs-lookup"><span data-stu-id="55262-232">a.</span></span> <span data-ttu-id="55262-233">A **E-mail** szöveg mezőbe írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="55262-233">In **E-mail** text box, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="55262-234">b.</span><span class="sxs-lookup"><span data-stu-id="55262-234">b.</span></span> <span data-ttu-id="55262-235">A **Utónév** szöveg mezőbe írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="55262-235">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="55262-236">c.</span><span class="sxs-lookup"><span data-stu-id="55262-236">c.</span></span> <span data-ttu-id="55262-237">A **Vezetéknév** szöveg mezőbe írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="55262-237">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

7. <span data-ttu-id="55262-238">Kattintson a **mentése tag** gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-238">Click **Save Member** button.</span></span>  

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="55262-240">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="55262-240">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="55262-241">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWdesk megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="55262-241">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWdesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="55262-243">**tooassign Britta Simon tooWdesk, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="55262-243">**tooassign Britta Simon tooWdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="55262-244">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55262-244">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="55262-246">Hello alkalmazások listában válassza ki a **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="55262-246">In hello applications list, select **Wdesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. <span data-ttu-id="55262-248">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="55262-248">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="55262-250">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="55262-250">Click **Add** button.</span></span> <span data-ttu-id="55262-251">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55262-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="55262-253">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="55262-253">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="55262-254">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55262-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="55262-255">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55262-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="55262-256">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="55262-256">Testing single sign-on</span></span>

<span data-ttu-id="55262-257">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="55262-257">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="55262-258">Ha a hozzáférési Panel hello hello Wdesk csempe gombra kattint, automatikusan bejelentkezett tooyour Wdesk alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="55262-258">When you click hello Wdesk tile in hello Access Panel, you should get automatically signed-on tooyour Wdesk application.</span></span>
<span data-ttu-id="55262-259">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="55262-259">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="55262-260">További források</span><span class="sxs-lookup"><span data-stu-id="55262-260">Additional resources</span></span>

* [<span data-ttu-id="55262-261">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="55262-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="55262-262">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="55262-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

