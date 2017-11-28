---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix ShareFile |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Citrix ShareFile között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: d7eaf140e56c40f9f621062849dd8558588ffd1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="f1965-103">Oktatóanyag: Azure Active Directoryval integrált Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="f1965-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="f1965-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Citrix ShareFile az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f1965-104">In this tutorial, you learn how toointegrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f1965-105">Citrix ShareFile integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f1965-105">Integrating Citrix ShareFile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f1965-106">Az Azure AD hozzáférési tooCitrix ShareFile rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="f1965-106">You can control in Azure AD who has access tooCitrix ShareFile.</span></span>
- <span data-ttu-id="f1965-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCitrix ShareFile (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="f1965-107">You can enable your users tooautomatically get signed-on tooCitrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f1965-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="f1965-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f1965-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f1965-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1965-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1965-110">Prerequisites</span></span>

<span data-ttu-id="f1965-111">az Azure AD-integráció a Citrix ShareFile tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f1965-111">tooconfigure Azure AD integration with Citrix ShareFile, you need hello following items:</span></span>

- <span data-ttu-id="f1965-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f1965-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f1965-113">A Citrix ShareFile egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f1965-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f1965-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f1965-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f1965-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f1965-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f1965-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1965-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f1965-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1965-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f1965-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f1965-118">Scenario description</span></span>
<span data-ttu-id="f1965-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f1965-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f1965-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f1965-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f1965-121">Adja hozzá a Citrix ShareFile hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f1965-121">Add Citrix ShareFile from hello gallery</span></span>
2. <span data-ttu-id="f1965-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1965-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-hello-gallery"></a><span data-ttu-id="f1965-123">Adja hozzá a Citrix ShareFile hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f1965-123">Add Citrix ShareFile from hello gallery</span></span>
<span data-ttu-id="f1965-124">tooconfigure hello integrációja Citrix ShareFile az Azure AD-be, meg kell tooadd Citrix ShareFile hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f1965-124">tooconfigure hello integration of Citrix ShareFile into Azure AD, you need tooadd Citrix ShareFile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f1965-125">**Citrix ShareFile hello gyűjteményből tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f1965-125">**tooadd Citrix ShareFile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1965-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f1965-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="f1965-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f1965-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f1965-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f1965-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="f1965-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f1965-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="f1965-133">Hello keresési mezőbe, írja be a **Citrix ShareFile**, jelölje be **Citrix ShareFile** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f1965-133">In hello search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Citrix ShareFile hello az eredmények listájában](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f1965-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1965-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f1965-136">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Citrix ShareFile "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f1965-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f1965-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Citrix ShareFile tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f1965-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Citrix ShareFile is tooa user in Azure AD.</span></span> <span data-ttu-id="f1965-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Citrix ShareFile közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="f1965-138">In other words, a link relationship between an Azure AD user and hello related user in Citrix ShareFile needs toobe established.</span></span>

<span data-ttu-id="f1965-139">Citrix ShareFile, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f1965-139">In Citrix ShareFile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f1965-140">tooconfigure és a Citrix ShareFile az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f1965-140">tooconfigure and test Azure AD single sign-on with Citrix ShareFile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f1965-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f1965-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f1965-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1965-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f1965-143">**[Citrix ShareFile tesztfelhasználó létrehozása](#create-a-citrix-sharefile-test-user)**  -toohave egy megfelelője a Britta Simon a Citrix ShareFile, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f1965-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - toohave a counterpart of Britta Simon in Citrix ShareFile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f1965-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f1965-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f1965-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f1965-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f1965-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1965-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f1965-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Citrix ShareFile alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="f1965-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="f1965-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Citrix ShareFile, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f1965-148">**tooconfigure Azure AD single sign-on with Citrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1965-149">Az Azure portál, a hello hello **Citrix ShareFile** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f1965-149">In hello Azure portal, on hello **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="f1965-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f1965-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="f1965-153">A hello **Citrix ShareFile tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1965-153">On hello **Citrix ShareFile Domain and URLs** section, perform hello following steps:</span></span>

    ![Citrix ShareFile tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="f1965-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="f1965-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f1965-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="f1965-156">This value is not real.</span></span> <span data-ttu-id="f1965-157">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f1965-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="f1965-158">Ügyfél [Citrix ShareFile ügyfél-támogatási csoport](https://www.citrix.co.in/products/sharefile/support.html) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="f1965-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) tooget this value.</span></span> 

4. <span data-ttu-id="f1965-159">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f1965-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="f1965-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1965-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f1965-163">A hello **Citrix ShareFile konfigurációs** kattintson **konfigurálása a Citrix ShareFile** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f1965-163">On hello **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f1965-164">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f1965-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Citrix ShareFile konfiguráció](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="f1965-166">Egy másik webes böngészőablakban, jelentkezzen be a **Citrix ShareFile** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f1965-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="f1965-167">Hello hello felső eszköztárán kattintson **Admin**.</span><span class="sxs-lookup"><span data-stu-id="f1965-167">In hello toolbar on hello top, click **Admin**.</span></span>

9. <span data-ttu-id="f1965-168">Hello bal oldali navigációs panelen, jelölje ki a **konfigurálása egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="f1965-168">In hello left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="f1965-169">![Felügyeleti fiók](./media/active-directory-saas-sharefile-tutorial/ic773627.png "felügyeleti fiók")</span><span class="sxs-lookup"><span data-stu-id="f1965-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="f1965-170">A hello **egyszeri bejelentkezés / SAML 2.0 konfigurációs** párbeszédpanel lapján **alapbeállítások**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1965-170">On hello **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform hello following steps:</span></span>
   
    <span data-ttu-id="f1965-171">![Egyszeri bejelentkezés](./media/active-directory-saas-sharefile-tutorial/ic773628.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="f1965-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="f1965-172">a.</span><span class="sxs-lookup"><span data-stu-id="f1965-172">a.</span></span> <span data-ttu-id="f1965-173">Kattintson a **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="f1965-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="f1965-174">b.</span><span class="sxs-lookup"><span data-stu-id="f1965-174">b.</span></span> <span data-ttu-id="f1965-175">A **a kiállító terjesztési hely kibocsátó / Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f1965-175">In **Your IDP Issuer/ Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f1965-176">c.</span><span class="sxs-lookup"><span data-stu-id="f1965-176">c.</span></span> <span data-ttu-id="f1965-177">Kattintson a **módosítás** következő toohello **X.509 tanúsítvány** mezőben, majd a feltöltési hello tanúsítványt töltötte le hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f1965-177">Click **Change** next toohello **X.509 Certificate** field and then upload hello certificate you downloaded from hello Azure portal.</span></span>
    
    <span data-ttu-id="f1965-178">d.</span><span class="sxs-lookup"><span data-stu-id="f1965-178">d.</span></span> <span data-ttu-id="f1965-179">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f1965-179">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="f1965-180">e.</span><span class="sxs-lookup"><span data-stu-id="f1965-180">e.</span></span> <span data-ttu-id="f1965-181">A **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f1965-181">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="f1965-182">Kattintson a **mentése** a hello Citrix ShareFile felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="f1965-182">Click **Save** on hello Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="f1965-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f1965-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f1965-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f1965-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f1965-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f1965-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f1965-186">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f1965-186">Create an Azure AD test user</span></span>

<span data-ttu-id="f1965-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f1965-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="f1965-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f1965-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1965-190">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1965-190">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f1965-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f1965-192">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f1965-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f1965-194">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f1965-196">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1965-196">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f1965-198">a.</span><span class="sxs-lookup"><span data-stu-id="f1965-198">a.</span></span> <span data-ttu-id="f1965-199">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f1965-199">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f1965-200">b.</span><span class="sxs-lookup"><span data-stu-id="f1965-200">b.</span></span> <span data-ttu-id="f1965-201">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="f1965-201">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f1965-202">c.</span><span class="sxs-lookup"><span data-stu-id="f1965-202">c.</span></span> <span data-ttu-id="f1965-203">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f1965-203">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f1965-204">d.</span><span class="sxs-lookup"><span data-stu-id="f1965-204">d.</span></span> <span data-ttu-id="f1965-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1965-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="f1965-206">Citrix ShareFile tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1965-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="f1965-207">A sorrend tooenable az Azure AD felhasználók toolog Citrix ShareFile be azok ki kell építenie a Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="f1965-207">In order tooenable Azure AD users toolog into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="f1965-208">Citrix ShareFile hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f1965-208">In hello case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="f1965-209">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f1965-209">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1965-210">Jelentkezzen be tooyour **Citrix ShareFile** bérlő.</span><span class="sxs-lookup"><span data-stu-id="f1965-210">Log in tooyour **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="f1965-211">Kattintson a **felhasználók kezelése \> otthoni felhasználók kezelése \> + alkalmazott létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f1965-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="f1965-212">![Alkalmazott létrehozása](./media/active-directory-saas-sharefile-tutorial/IC781050.png "alkalmazott létrehozása")</span><span class="sxs-lookup"><span data-stu-id="f1965-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="f1965-213">A hello **alapvető információkat** csoportjában hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="f1965-213">On hello **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="f1965-214">![Alapvető információkat](./media/active-directory-saas-sharefile-tutorial/IC799951.png "alapvető információk")</span><span class="sxs-lookup"><span data-stu-id="f1965-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="f1965-215">a.</span><span class="sxs-lookup"><span data-stu-id="f1965-215">a.</span></span> <span data-ttu-id="f1965-216">A hello **E-mail cím** szövegmezőhöz típus hello e-mail címét, Britta Simon  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="f1965-216">In hello **Email Address** textbox, type hello email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="f1965-217">b.</span><span class="sxs-lookup"><span data-stu-id="f1965-217">b.</span></span> <span data-ttu-id="f1965-218">A hello **Utónév** szövegmezőhöz típus **Utónév** , felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="f1965-218">In hello **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="f1965-219">c.</span><span class="sxs-lookup"><span data-stu-id="f1965-219">c.</span></span> <span data-ttu-id="f1965-220">A hello **Vezetéknév** szövegmezőhöz típus **Vezetéknév** , felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="f1965-220">In hello **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="f1965-221">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f1965-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="f1965-222">hello Azure AD fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik. Bármely más Citrix ShareFile felhasználói fiók létrehozása eszközök vagy Citrix ShareFile tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="f1965-222">hello Azure AD account holder will receive an email and follow a link tooconfirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f1965-223">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="f1965-223">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f1965-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCitrix ShareFile megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f1965-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCitrix ShareFile.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="f1965-226">**tooassign Britta Simon tooCitrix ShareFile, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f1965-226">**tooassign Britta Simon tooCitrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1965-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f1965-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f1965-229">Hello alkalmazások listában válassza ki a **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="f1965-229">In hello applications list, select **Citrix ShareFile**.</span></span>

    ![hello hello alkalmazások listáját a Citrix ShareFile hivatkozás](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="f1965-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f1965-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="f1965-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1965-233">Click **Add** button.</span></span> <span data-ttu-id="f1965-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1965-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="f1965-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f1965-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f1965-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1965-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f1965-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f1965-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f1965-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f1965-239">Test single sign-on</span></span>

<span data-ttu-id="f1965-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f1965-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f1965-241">Hello Citrix ShareFile hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Citrix ShareFile alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f1965-241">When you click hello Citrix ShareFile tile in hello Access Panel, you should get automatically signed-on tooyour Citrix ShareFile application.</span></span>
<span data-ttu-id="f1965-242">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f1965-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f1965-243">További források</span><span class="sxs-lookup"><span data-stu-id="f1965-243">Additional resources</span></span>

* [<span data-ttu-id="f1965-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f1965-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f1965-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f1965-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

