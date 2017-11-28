---
title: "Oktatóanyag: Azure Active Directoryval integrált Datahug |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Datahug között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: 79ccb939c7a19720bcf696af141f747db00c8a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="8ba1c-103">Oktatóanyag: Azure Active Directoryval integrált Datahug</span><span class="sxs-lookup"><span data-stu-id="8ba1c-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="8ba1c-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Datahug az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ba1c-104">In this tutorial, you learn how toointegrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ba1c-105">Datahug integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-105">Integrating Datahug with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8ba1c-106">Megadhatja a hozzáférés tooDatahug rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8ba1c-106">You can control in Azure AD who has access tooDatahug</span></span>
- <span data-ttu-id="8ba1c-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDatahug (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8ba1c-107">You can enable your users tooautomatically get signed-on tooDatahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ba1c-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8ba1c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8ba1c-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="8ba1c-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ba1c-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ba1c-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ba1c-111">Prerequisites</span></span>

<span data-ttu-id="8ba1c-112">az Azure AD integrálása Datahug tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-112">tooconfigure Azure AD integration with Datahug, you need hello following items:</span></span>

- <span data-ttu-id="8ba1c-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8ba1c-113">An Azure AD subscription</span></span>
- <span data-ttu-id="8ba1c-114">Egy Datahug egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="8ba1c-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ba1c-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ba1c-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ba1c-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ba1c-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ba1c-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ba1c-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-119">Scenario description</span></span>
<span data-ttu-id="8ba1c-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ba1c-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ba1c-122">Hello gyűjteményből Datahug hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-122">Adding Datahug from hello gallery</span></span>
2. <span data-ttu-id="8ba1c-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8ba1c-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-hello-gallery"></a><span data-ttu-id="8ba1c-124">Hello gyűjteményből Datahug hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-124">Adding Datahug from hello gallery</span></span>
<span data-ttu-id="8ba1c-125">tooconfigure hello integrációja Datahug az Azure AD-be, meg kell tooadd Datahug hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-125">tooconfigure hello integration of Datahug into Azure AD, you need tooadd Datahug from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8ba1c-126">**tooadd Datahug hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-126">**tooadd Datahug from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ba1c-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ba1c-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8ba1c-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8ba1c-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8ba1c-134">Hello keresési mezőbe, írja be a **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-134">In hello search box, type **Datahug**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="8ba1c-136">A hello eredmények panelen válassza ki a **Datahug**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-136">In hello results panel, select **Datahug**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ba1c-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8ba1c-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ba1c-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Datahug</span><span class="sxs-lookup"><span data-stu-id="8ba1c-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8ba1c-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Datahug tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Datahug is tooa user in Azure AD.</span></span> <span data-ttu-id="8ba1c-141">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Datahug közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-141">In other words, a link relationship between an Azure AD user and hello related user in Datahug needs toobe established.</span></span>

<span data-ttu-id="8ba1c-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Datahug.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Datahug.</span></span>

<span data-ttu-id="8ba1c-143">tooconfigure és az Azure AD az egyszeri bejelentkezés Datahug-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-143">tooconfigure and test Azure AD single sign-on with Datahug, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8ba1c-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8ba1c-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ba1c-146">**[Datahug tesztfelhasználó létrehozása](#creating-a-datahug-test-user)**  -toohave egy megfelelője a Britta Simon a Datahug, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - toohave a counterpart of Britta Simon in Datahug that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ba1c-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ba1c-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ba1c-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ba1c-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Datahug alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="8ba1c-151">**az Azure AD tooconfigure egyszeri bejelentkezést a Datahug, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-151">**tooconfigure Azure AD single sign-on with Datahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ba1c-152">Az Azure portál, a hello hello **Datahug** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-152">In hello Azure portal, on hello **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8ba1c-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="8ba1c-156">A hello **Datahug tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-156">On hello **Datahug Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="8ba1c-158">a.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-158">a.</span></span> <span data-ttu-id="8ba1c-159">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="8ba1c-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="8ba1c-160">b.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-160">b.</span></span> <span data-ttu-id="8ba1c-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="8ba1c-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="8ba1c-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="8ba1c-163">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-163">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="8ba1c-165">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="8ba1c-165">In hello **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="8ba1c-166">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-166">These values are not hello real.</span></span> <span data-ttu-id="8ba1c-167">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-167">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="8ba1c-168">Itt javasoljuk, hogy toouse hello egyedi érték hello azonosító karakterláncot, és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-168">Here we suggest you toouse hello unique value of string in hello Identifier and Reply URL.</span></span> <span data-ttu-id="8ba1c-169">Ügyfél [Datahug ügyfél-támogatási csoport](http://datahug.com/about/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) tooget these values.</span></span> 

5. <span data-ttu-id="8ba1c-170">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="8ba1c-172">Ellenőrizze **"Megjelenítése speciális tanúsítvány-aláírási beállításai"** , és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-172">Check **“Show advanced certificate signing settings”** and perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="8ba1c-174">a.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-174">a.</span></span> <span data-ttu-id="8ba1c-175">A **aláíró beállítás**, jelölje be **bejelentkezési SAML-előfeltétel**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="8ba1c-176">b.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-176">b.</span></span> <span data-ttu-id="8ba1c-177">A **aláíró algoritmus**, jelölje be **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="8ba1c-178">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-178">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="8ba1c-180">A hello **Datahug konfigurációs** kattintson **konfigurálása Datahug** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-180">On hello **Datahug Configuration** section, click **Configure Datahug** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8ba1c-181">Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-181">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="8ba1c-183">tooconfigure egyszeri bejelentkezést a **Datahug** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** túl[Datahug támogatási](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="8ba1c-183">tooconfigure single sign-on on **Datahug** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="8ba1c-184">Maguk állítják be a toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva fel ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-184">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8ba1c-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8ba1c-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8ba1c-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8ba1c-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ba1c-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ba1c-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ba1c-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8ba1c-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ba1c-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ba1c-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ba1c-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ba1c-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8ba1c-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ba1c-200">a.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-200">a.</span></span> <span data-ttu-id="8ba1c-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ba1c-202">b.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-202">b.</span></span> <span data-ttu-id="8ba1c-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ba1c-204">c.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-204">c.</span></span> <span data-ttu-id="8ba1c-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8ba1c-206">d.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-206">d.</span></span> <span data-ttu-id="8ba1c-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="8ba1c-208">Datahug tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ba1c-208">Creating a Datahug test user</span></span>

<span data-ttu-id="8ba1c-209">az Azure AD tooenable felhasználók toolog a tooDatahug, akkor ki kell építenie Datahug be.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-209">tooenable Azure AD users toolog in tooDatahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="8ba1c-210">Datahug, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="8ba1c-211">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ba1c-212">Jelentkezzen be tooyour Datahug vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-212">Log in tooyour Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="8ba1c-213">Hello rámutat **fogaskerékre** hello jobb felső sarkában, majd kattintson a **beállítások**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-213">Hover over hello **cog** in hello top right-hand corner and click **Settings**</span></span>
   
   ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="8ba1c-215">Válasszon **személyek** hello kattintson **felhasználó hozzáadása** lap</span><span class="sxs-lookup"><span data-stu-id="8ba1c-215">Choose **People** and click hello **Add Users** tab</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="8ba1c-217">Írja be a hello személy hello e-mailek toocreate egy fiókot, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-217">Type hello email of hello person you would like toocreate an account for and click **Add**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="8ba1c-219">Regisztrációs mail toouser kiválasztásával elküldheti **üdvözlő e-mailek küldése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-219">You can send registration mail toouser by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="8ba1c-220">Létrehozásakor egy Salesforce fiókot ne küldjön hello üdvözlő e-mailt.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-220">If you are creating an account for Salesforce do not send hello welcome email.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8ba1c-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8ba1c-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8ba1c-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDatahug megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDatahug.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8ba1c-224">**tooassign Britta Simon tooDatahug, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8ba1c-224">**tooassign Britta Simon tooDatahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ba1c-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8ba1c-227">Hello alkalmazások listában válassza ki a **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-227">In hello applications list, select **Datahug**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="8ba1c-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8ba1c-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-231">Click **Add** button.</span></span> <span data-ttu-id="8ba1c-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8ba1c-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8ba1c-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ba1c-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ba1c-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8ba1c-237">Testing single sign-on</span></span>

<span data-ttu-id="8ba1c-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="8ba1c-239">Ha a hozzáférési Panel hello hello Datahug csempe gombra kattint, automatikusan bejelentkezett tooyour Datahug alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="8ba1c-239">When you click hello Datahug tile in hello Access Panel, you should get automatically signed-on tooyour Datahug application.</span></span> <span data-ttu-id="8ba1c-240">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="8ba1c-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8ba1c-241">További források</span><span class="sxs-lookup"><span data-stu-id="8ba1c-241">Additional resources</span></span>

* [<span data-ttu-id="8ba1c-242">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8ba1c-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ba1c-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8ba1c-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

