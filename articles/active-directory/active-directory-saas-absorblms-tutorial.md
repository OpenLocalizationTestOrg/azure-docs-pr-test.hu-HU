---
title: "Oktatóanyag: Azure Active Directoryval integrált felvegye LMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és felvegye LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a><span data-ttu-id="21682-103">Oktatóanyag: Azure Active Directoryval integrált felvegye LMS</span><span class="sxs-lookup"><span data-stu-id="21682-103">Tutorial: Azure Active Directory integration with Absorb LMS</span></span>

<span data-ttu-id="21682-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Absorb LMS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="21682-104">In this tutorial, you learn how toointegrate Absorb LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21682-105">Felvegye LMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="21682-105">Integrating Absorb LMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="21682-106">Megadhatja a hozzáférés tooAbsorb LMS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="21682-106">You can control in Azure AD who has access tooAbsorb LMS</span></span>
- <span data-ttu-id="21682-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAbsorb LMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="21682-107">You can enable your users tooautomatically get signed-on tooAbsorb LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="21682-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="21682-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="21682-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="21682-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="21682-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="21682-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21682-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21682-111">Prerequisites</span></span>

<span data-ttu-id="21682-112">tooconfigure felvegye LMS az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="21682-112">tooconfigure Azure AD integration with Absorb LMS, you need hello following items:</span></span>

- <span data-ttu-id="21682-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="21682-113">An Azure AD subscription</span></span>
- <span data-ttu-id="21682-114">Egy felvegye LMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="21682-114">An Absorb LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="21682-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="21682-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="21682-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="21682-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="21682-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="21682-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="21682-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="21682-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="21682-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="21682-119">Scenario description</span></span>
<span data-ttu-id="21682-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="21682-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="21682-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="21682-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="21682-122">Hello gyűjteményből felvegye LMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21682-122">Adding Absorb LMS from hello gallery</span></span>
2. <span data-ttu-id="21682-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21682-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-absorb-lms-from-hello-gallery"></a><span data-ttu-id="21682-124">Hello gyűjteményből felvegye LMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21682-124">Adding Absorb LMS from hello gallery</span></span>
<span data-ttu-id="21682-125">tooconfigure hello integrációja LMS felvegye a tooAzure AD, meg kell tooadd Absorb LMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="21682-125">tooconfigure hello integration of Absorb LMS in tooAzure AD, you need tooadd Absorb LMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="21682-126">**tooadd Absorb LMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="21682-126">**tooadd Absorb LMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="21682-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="21682-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="21682-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="21682-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="21682-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="21682-130">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="21682-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="21682-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="21682-134">Hello keresési mezőbe, írja be a **felvegye LMS**, jelölje be **felvegye LMS** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="21682-134">In hello search box, type **Absorb LMS**, select **Absorb LMS** from result panel then click **Add** button tooadd hello application.</span></span>

    ![LMS felvegye a hello eredményeinek listája](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="21682-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21682-136">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="21682-137">Ebben a szakaszban konfigurálhatja, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján felvegye LMS</span><span class="sxs-lookup"><span data-stu-id="21682-137">In this section, you configure and test Azure AD single sign-on with Absorb LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="21682-138">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LMS felvegye a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="21682-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Absorb LMS is tooa user in Azure AD.</span></span> <span data-ttu-id="21682-139">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LMS felvegye a hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="21682-139">In other words, a link relationship between an Azure AD user and hello related user in Absorb LMS needs toobe established.</span></span>

<span data-ttu-id="21682-140">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LMS felvegye a.</span><span class="sxs-lookup"><span data-stu-id="21682-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Absorb LMS.</span></span>

<span data-ttu-id="21682-141">tooconfigure és az Azure AD az egyszeri bejelentkezés felvegye LMS-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="21682-141">tooconfigure and test Azure AD single sign-on with Absorb LMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="21682-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21682-142">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="21682-143">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21682-143">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="21682-144">**[Hozzon létre egy felvegye LMS tesztfelhasználó](#create-an-absorb-lms-test-user)**  -toohave egy megfelelője a Britta Simon a LMS felvegye, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="21682-144">**[Create an Absorb LMS test user](#create-an-absorb-lms-test-user)** - toohave a counterpart of Britta Simon in Absorb LMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="21682-145">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21682-145">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="21682-146">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="21682-146">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="21682-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21682-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="21682-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az felvegye LMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="21682-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Absorb LMS application.</span></span>

<span data-ttu-id="21682-149">**az Azure AD tooconfigure egyszeri bejelentkezés LMS felvegye a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="21682-149">**tooconfigure Azure AD single sign-on with Absorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="21682-150">Az Azure portál, a hello hello **felvegye LMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="21682-150">In hello Azure portal, on hello **Absorb LMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="21682-152">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21682-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. <span data-ttu-id="21682-154">A hello **felvegye LMS tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21682-154">On hello **Absorb LMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Felvegye LMS tartomány és az URL-címek egyetlen bejelentkezés információk](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    <span data-ttu-id="21682-156">a.</span><span class="sxs-lookup"><span data-stu-id="21682-156">a.</span></span> <span data-ttu-id="21682-157">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="21682-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>

    <span data-ttu-id="21682-158">b.</span><span class="sxs-lookup"><span data-stu-id="21682-158">b.</span></span> <span data-ttu-id="21682-159">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="21682-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="21682-160">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="21682-160">These values are not hello real.</span></span> <span data-ttu-id="21682-161">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="21682-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="21682-162">Ügyfél [LMS ügyfél felvegye támogatási csoport](https://www.absorblms.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="21682-162">Contact [Absorb LMS Client support team](https://www.absorblms.com/support) tooget these values.</span></span> 

4. <span data-ttu-id="21682-163">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="21682-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. <span data-ttu-id="21682-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="21682-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="21682-167">A hello **felvegye LMS konfigurációs** területén kattintson **felvegye LMS konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="21682-167">On hello **Absorb LMS Configuration** section, click **Configure Absorb LMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="21682-168">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="21682-168">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Felvegye LMS konfiguráció](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. <span data-ttu-id="21682-170">Egy másik webes böngészőablakban jelentkezzen tooyour felvegye LMS vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="21682-170">In a different web browser window, log in tooyour Absorb LMS company site as an administrator.</span></span>

9. <span data-ttu-id="21682-171">Kattintson a hello **fiók ikon** hello rendszergazdai kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="21682-171">Click hello **Account Icon** on hello admin interface.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/1.png)

10. <span data-ttu-id="21682-173">Kattintson a **portálbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="21682-173">Click **Portal Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. <span data-ttu-id="21682-175">Kattintson a hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="21682-175">Click hello **Users** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/3.png)

12. <span data-ttu-id="21682-177">Hajtsa végre a következő lépéseket tooaccess hello egyszeri bejelentkezés konfigurációs mezők hello:</span><span class="sxs-lookup"><span data-stu-id="21682-177">Perform hello following steps tooaccess hello Single Sign-On configuration fields:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/4.png)

    <span data-ttu-id="21682-179">a.</span><span class="sxs-lookup"><span data-stu-id="21682-179">a.</span></span> <span data-ttu-id="21682-180">Jelölje be hello megfelelő **mód**.</span><span class="sxs-lookup"><span data-stu-id="21682-180">Select hello appropriate **Mode**.</span></span>

    <span data-ttu-id="21682-181">b.</span><span class="sxs-lookup"><span data-stu-id="21682-181">b.</span></span> <span data-ttu-id="21682-182">Nyissa meg hello hello Azure-portálon a Jegyzettömbben a letöltött tanúsítvány eltávolítása hello **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a fennmaradó hello Hello **kulcs** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21682-182">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Key** textbox.</span></span>
    
    <span data-ttu-id="21682-183">c.</span><span class="sxs-lookup"><span data-stu-id="21682-183">c.</span></span> <span data-ttu-id="21682-184">A hello **azonosítóját megadó tulajdonságot**, válassza ki a megfelelő attribútumot, amely konfigurált az hello (például ha hello userprinciplename az Azure AD-van kiválasztva, majd a felhasználónév itt választott lenne.) az Azure AD a felhasználói azonosító hello hello</span><span class="sxs-lookup"><span data-stu-id="21682-184">In hello **Id Property**, select hello appropriate attribute which you have configured as hello user identifier in hello Azure AD (For example, If hello userprinciplename is selected in Azure AD, then Username would be selected here.)</span></span>

    <span data-ttu-id="21682-185">d.</span><span class="sxs-lookup"><span data-stu-id="21682-185">d.</span></span> <span data-ttu-id="21682-186">A hello **bejelentkezési URL-cím**, illessze be a hello **"SAML-alapú egyszeri bejelentkezési URL-címe"** hello a másolt érték **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="21682-186">In hello **Login URL**, paste hello **“SAML Single Sign-On Service URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="21682-187">e.</span><span class="sxs-lookup"><span data-stu-id="21682-187">e.</span></span> <span data-ttu-id="21682-188">A hello **kijelentkezési URL-cím**, illessze be a hello **"Sign-Out URL-címe"** hello a másolt érték **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="21682-188">In hello **Logout URL**, paste hello **“Sign-Out URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

13. <span data-ttu-id="21682-189">Engedélyezése **"Csak engedélyezése Egyszeri bejelentkezéshez"**.</span><span class="sxs-lookup"><span data-stu-id="21682-189">Enable **‘Only Allow SSO Login’**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/5.png)

14. <span data-ttu-id="21682-191">Kattintson a **"Mentés".**</span><span class="sxs-lookup"><span data-stu-id="21682-191">Click **"Save."**</span></span>

> [!TIP]
> <span data-ttu-id="21682-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="21682-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="21682-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="21682-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="21682-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="21682-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="21682-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="21682-195">Create an Azure AD test user</span></span>

<span data-ttu-id="21682-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="21682-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="21682-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="21682-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="21682-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="21682-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="21682-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="21682-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="21682-203">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21682-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="21682-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21682-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="21682-207">a.</span><span class="sxs-lookup"><span data-stu-id="21682-207">a.</span></span> <span data-ttu-id="21682-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="21682-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="21682-209">b.</span><span class="sxs-lookup"><span data-stu-id="21682-209">b.</span></span> <span data-ttu-id="21682-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="21682-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="21682-211">c.</span><span class="sxs-lookup"><span data-stu-id="21682-211">c.</span></span> <span data-ttu-id="21682-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="21682-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="21682-213">d.</span><span class="sxs-lookup"><span data-stu-id="21682-213">d.</span></span> <span data-ttu-id="21682-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="21682-214">Click **Create**.</span></span>

### <a name="create-an-absorb-lms-test-user"></a><span data-ttu-id="21682-215">Hozzon létre egy felvegye LMS tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="21682-215">Create an Absorb LMS test user</span></span>

<span data-ttu-id="21682-216">az Azure AD tooenable felhasználók toolog tooAbsorb LMS, a ezeket ki kell építenie a tooAbsorb LMS.</span><span class="sxs-lookup"><span data-stu-id="21682-216">tooenable Azure AD users toolog in tooAbsorb LMS, they must be provisioned in tooAbsorb LMS.</span></span>  
<span data-ttu-id="21682-217">LMS felvegye a kiépítés kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="21682-217">For Absorb LMS, provisioning is a manual task.</span></span>

<span data-ttu-id="21682-218">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="21682-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="21682-219">Jelentkezzen be tooyour felvegye LMS vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="21682-219">Log in tooyour Absorb LMS company site as an administrator.</span></span>

2. <span data-ttu-id="21682-220">Kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="21682-220">Click **Users** tab.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. <span data-ttu-id="21682-222">Kattintson a **felhasználók** alatt hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="21682-222">Click **Users** under hello **Users** tab.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  <span data-ttu-id="21682-224">Válassza ki **felhasználói** a **új hozzáadása** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="21682-224">Select **User** from **Add New** drop-down.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. <span data-ttu-id="21682-226">A hello **felhasználó hozzáadása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21682-226">On hello **Add User** page, perform hello following steps:</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/user.png)

    <span data-ttu-id="21682-228">a.</span><span class="sxs-lookup"><span data-stu-id="21682-228">a.</span></span> <span data-ttu-id="21682-229">A hello **Keresztnév** szövegmező, például Britta hello első neve.</span><span class="sxs-lookup"><span data-stu-id="21682-229">In hello **First Name** textbox, type hello first name like Britta.</span></span>

    <span data-ttu-id="21682-230">b.</span><span class="sxs-lookup"><span data-stu-id="21682-230">b.</span></span> <span data-ttu-id="21682-231">A hello **Vezetéknév** szövegmezőhöz hello utolsó típusnév Simon hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="21682-231">In hello **Last Name** textbox, type hello last name like Simon.</span></span>
    
    <span data-ttu-id="21682-232">c.</span><span class="sxs-lookup"><span data-stu-id="21682-232">c.</span></span> <span data-ttu-id="21682-233">A hello **felhasználónév** szövegmező, például Britta Simon hello felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="21682-233">In hello **Username** textbox, type hello user name like Britta Simon.</span></span>

    <span data-ttu-id="21682-234">d.</span><span class="sxs-lookup"><span data-stu-id="21682-234">d.</span></span> <span data-ttu-id="21682-235">A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="21682-235">In hello **Password** textbox, type hello password of Britta Simon.</span></span>

    <span data-ttu-id="21682-236">e.</span><span class="sxs-lookup"><span data-stu-id="21682-236">e.</span></span> <span data-ttu-id="21682-237">A hello **jelszó megerősítése** szövegmezőhöz típus hello ugyanazt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="21682-237">In hello **Confirm Password** textbox, type hello same password.</span></span>
    
    <span data-ttu-id="21682-238">f.</span><span class="sxs-lookup"><span data-stu-id="21682-238">f.</span></span> <span data-ttu-id="21682-239">Ügyeljen rá **aktív**.</span><span class="sxs-lookup"><span data-stu-id="21682-239">Make it as **ACTIVE**.</span></span>   

6. <span data-ttu-id="21682-240">Kattintson a **"Mentés".**</span><span class="sxs-lookup"><span data-stu-id="21682-240">Click **"Save."**</span></span>
 
### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="21682-241">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="21682-241">Assign hello Azure AD test user</span></span>

<span data-ttu-id="21682-242">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAbsorb LMS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="21682-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAbsorb LMS.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="21682-244">**tooassign Britta Simon tooAbsorb LMS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="21682-244">**tooassign Britta Simon tooAbsorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="21682-245">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="21682-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="21682-247">Hello alkalmazások listában válassza ki a **felvegye LMS**.</span><span class="sxs-lookup"><span data-stu-id="21682-247">In hello applications list, select **Absorb LMS**.</span></span>

    ![hello felvegye LMS hivatkozásra hello alkalmazások listája](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. <span data-ttu-id="21682-249">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="21682-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="21682-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="21682-251">Click **Add** button.</span></span> <span data-ttu-id="21682-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21682-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="21682-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="21682-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="21682-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21682-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="21682-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21682-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="21682-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="21682-257">Test single sign-on</span></span>

<span data-ttu-id="21682-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="21682-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="21682-259">Hello LMS felvegye a hozzáférési Panel hello csempén kattintson automatikusan bejelentkezett tooyour felvegye LMS alkalmazás fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="21682-259">Click hello Absorb LMS tile in hello Access Panel, you will get automatically signed-on tooyour Absorb LMS application.</span></span> <span data-ttu-id="21682-260">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="21682-260">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21682-261">További források</span><span class="sxs-lookup"><span data-stu-id="21682-261">Additional resources</span></span>

* [<span data-ttu-id="21682-262">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="21682-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="21682-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="21682-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

