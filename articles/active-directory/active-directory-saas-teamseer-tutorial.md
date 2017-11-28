---
title: "Oktatóanyag: Azure Active Directoryval integrált TeamSeer |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TeamSeer között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="0e46a-103">Oktatóanyag: Azure Active Directoryval integrált TeamSeer</span><span class="sxs-lookup"><span data-stu-id="0e46a-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="0e46a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TeamSeer az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0e46a-104">In this tutorial, you learn how toointegrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0e46a-105">TeamSeer integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-105">Integrating TeamSeer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0e46a-106">Megadhatja a hozzáférés tooTeamSeer rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0e46a-106">You can control in Azure AD who has access tooTeamSeer</span></span>
- <span data-ttu-id="0e46a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTeamSeer (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0e46a-107">You can enable your users tooautomatically get signed-on tooTeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0e46a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0e46a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0e46a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0e46a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e46a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0e46a-110">Prerequisites</span></span>

<span data-ttu-id="0e46a-111">az Azure AD integrálása TeamSeer tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-111">tooconfigure Azure AD integration with TeamSeer, you need hello following items:</span></span>

- <span data-ttu-id="0e46a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0e46a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0e46a-113">Egy TeamSeer egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0e46a-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0e46a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0e46a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0e46a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0e46a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0e46a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0e46a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0e46a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e46a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0e46a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0e46a-118">Scenario description</span></span>
<span data-ttu-id="0e46a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0e46a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0e46a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0e46a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0e46a-121">Hello gyűjteményből TeamSeer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0e46a-121">Adding TeamSeer from hello gallery</span></span>
2. <span data-ttu-id="0e46a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0e46a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-hello-gallery"></a><span data-ttu-id="0e46a-123">Hello gyűjteményből TeamSeer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0e46a-123">Adding TeamSeer from hello gallery</span></span>
<span data-ttu-id="0e46a-124">tooconfigure hello integrációja TeamSeer tooAzure AD a, meg kell tooadd TeamSeer hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0e46a-124">tooconfigure hello integration of TeamSeer in tooAzure AD, you need tooadd TeamSeer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0e46a-125">**tooadd TeamSeer hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0e46a-125">**tooadd TeamSeer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e46a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0e46a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0e46a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0e46a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0e46a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0e46a-133">Hello keresési mezőbe, írja be a **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-133">In hello search box, type **TeamSeer**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="0e46a-135">A hello eredmények panelen válassza ki a **TeamSeer**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-135">In hello results panel, select **TeamSeer**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0e46a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0e46a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0e46a-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján TeamSeer</span><span class="sxs-lookup"><span data-stu-id="0e46a-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0e46a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TeamSeer tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0e46a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TeamSeer is tooa user in Azure AD.</span></span> <span data-ttu-id="0e46a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TeamSeer közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0e46a-140">In other words, a link relationship between an Azure AD user and hello related user in TeamSeer needs toobe established.</span></span>

<span data-ttu-id="0e46a-141">TeamSeer, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0e46a-141">In TeamSeer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0e46a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés TeamSeer-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0e46a-142">tooconfigure and test Azure AD single sign-on with TeamSeer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0e46a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0e46a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0e46a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0e46a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0e46a-145">**[TeamSeer tesztfelhasználó létrehozása](#creating-a-teamseer-test-user)**  -toohave egy megfelelője a Britta Simon a TeamSeer, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="0e46a-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - toohave a counterpart of Britta Simon in TeamSeer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0e46a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0e46a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0e46a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0e46a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0e46a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0e46a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0e46a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TeamSeer alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0e46a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="0e46a-150">**az Azure AD tooconfigure egyszeri bejelentkezést a TeamSeer, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0e46a-150">**tooconfigure Azure AD single sign-on with TeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e46a-151">Az Azure portál, a hello hello **TeamSeer** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-151">In hello Azure portal, on hello **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0e46a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0e46a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="0e46a-155">A hello **TeamSeer tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-155">On hello **TeamSeer Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="0e46a-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="0e46a-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0e46a-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="0e46a-158">hello value is not real.</span></span> <span data-ttu-id="0e46a-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="0e46a-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="0e46a-160">Ügyfél [TeamSeer ügyfél-támogatási csoport](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="0e46a-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello value.</span></span> 
 
4. <span data-ttu-id="0e46a-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0e46a-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="0e46a-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0e46a-165">A hello **TeamSeer konfigurációs** kattintson **konfigurálása TeamSeer** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0e46a-165">On hello **TeamSeer Configuration** section, click **Configure TeamSeer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0e46a-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0e46a-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="0e46a-168">Egy másik webes böngészőablakban jelentkezzen tooyour TeamSeer vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0e46a-168">In a different web browser window, log in tooyour TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="0e46a-169">Nyissa meg túl**HR Admin**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-169">Go too**HR Admin**.</span></span>
   
    <span data-ttu-id="0e46a-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR-rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="0e46a-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="0e46a-171">Kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="0e46a-172">![A telepítő](./media/active-directory-saas-teamseer-tutorial/ic789635.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="0e46a-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="0e46a-173">Kattintson a **SAML szolgáltató részleteinek beállítása**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="0e46a-174">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="0e46a-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="0e46a-175">Hello SAML szolgáltató részletes adatait tartalmazó részben, a hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-175">In hello SAML provider details section, perform hello following steps:</span></span>
   
    <span data-ttu-id="0e46a-176">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="0e46a-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="0e46a-177">a.</span><span class="sxs-lookup"><span data-stu-id="0e46a-177">a.</span></span> <span data-ttu-id="0e46a-178">Beillesztés hello **egyszeri bejelentkezési URL-címe** toohello érték **URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0e46a-178">Paste hello **Single Sign-On Service URL** value in toohello **URL** textbox.</span></span>
          
    <span data-ttu-id="0e46a-179">b.</span><span class="sxs-lookup"><span data-stu-id="0e46a-179">b.</span></span> <span data-ttu-id="0e46a-180">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello tooyour vágólapon tartalma és toohello Beillesztés **IdP nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0e46a-180">Open your base-64 encoded certificate in notepad, copy hello content of it in tooyour clipboard, and then paste it toohello **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="0e46a-181">toocomplete hello SAML-szolgáltató konfigurációjának, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-181">toocomplete hello SAML provider configuration, perform hello following steps:</span></span>
    
    <span data-ttu-id="0e46a-182">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="0e46a-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="0e46a-183">a.</span><span class="sxs-lookup"><span data-stu-id="0e46a-183">a.</span></span> <span data-ttu-id="0e46a-184">A hello **teszt E-mail címek**, írja be a hello teszt felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="0e46a-184">In hello **Test Email Addresses**, type hello test user’s email address.</span></span> 
  
    <span data-ttu-id="0e46a-185">b.</span><span class="sxs-lookup"><span data-stu-id="0e46a-185">b.</span></span> <span data-ttu-id="0e46a-186">A hello **kibocsátó** szövegmezőhöz típus hello hello szolgáltató kibocsátó URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="0e46a-186">In hello **Issuer** textbox, type hello Issuer URL of hello service provider.</span></span> 
  
    <span data-ttu-id="0e46a-187">c.</span><span class="sxs-lookup"><span data-stu-id="0e46a-187">c.</span></span> <span data-ttu-id="0e46a-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0e46a-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0e46a-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0e46a-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0e46a-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0e46a-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0e46a-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0e46a-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e46a-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="0e46a-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0e46a-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0e46a-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0e46a-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e46a-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0e46a-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0e46a-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e46a-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0e46a-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0e46a-204">a.</span><span class="sxs-lookup"><span data-stu-id="0e46a-204">a.</span></span> <span data-ttu-id="0e46a-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0e46a-206">b.</span><span class="sxs-lookup"><span data-stu-id="0e46a-206">b.</span></span> <span data-ttu-id="0e46a-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0e46a-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0e46a-208">c.</span><span class="sxs-lookup"><span data-stu-id="0e46a-208">c.</span></span> <span data-ttu-id="0e46a-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0e46a-210">d.</span><span class="sxs-lookup"><span data-stu-id="0e46a-210">d.</span></span> <span data-ttu-id="0e46a-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="0e46a-212">TeamSeer tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e46a-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="0e46a-213">az Azure AD tooenable felhasználók toolog tooTeamSeer, az ezeket ki kell építenie a tooShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="0e46a-213">tooenable Azure AD users toolog in tooTeamSeer, they must be provisioned in tooShiftPlanning.</span></span> <span data-ttu-id="0e46a-214">TeamSeer hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="0e46a-214">In hello case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="0e46a-215">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0e46a-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e46a-216">Jelentkezzen be tooyour **TeamSeer** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0e46a-216">Log in tooyour **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="0e46a-217">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-217">Perform hello following steps:</span></span>
   
    <span data-ttu-id="0e46a-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR-rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="0e46a-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="0e46a-219">a.</span><span class="sxs-lookup"><span data-stu-id="0e46a-219">a.</span></span> <span data-ttu-id="0e46a-220">Nyissa meg túl**HR Admin \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-220">Go too**HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="0e46a-221">b.</span><span class="sxs-lookup"><span data-stu-id="0e46a-221">b.</span></span> <span data-ttu-id="0e46a-222">Kattintson a **hello új felhasználó varázsló futtatása**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-222">Click **Run hello New User wizard**.</span></span>

3. <span data-ttu-id="0e46a-223">A hello **felhasználó adatait** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e46a-223">In hello **User Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="0e46a-224">![Felhasználó adatait](./media/active-directory-saas-teamseer-tutorial/ic789641.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="0e46a-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="0e46a-225">a.</span><span class="sxs-lookup"><span data-stu-id="0e46a-225">a.</span></span> <span data-ttu-id="0e46a-226">Típus hello **Utónév**, **vezetékneve**, **felhasználónevet (E-mail címet)** a egy érvényes AAD-fiókba, azt szeretné, hogy a toohello tooprovision kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="0e46a-226">Type hello **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want tooprovision in toohello related textboxes.</span></span>
  
    <span data-ttu-id="0e46a-227">b.</span><span class="sxs-lookup"><span data-stu-id="0e46a-227">b.</span></span> <span data-ttu-id="0e46a-228">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-228">Click **Next**.</span></span>

4. <span data-ttu-id="0e46a-229">Hajtsa végre a hello képernyőn megjelenő utasításokat az új felhasználó hozzáadásához, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-229">Follow hello on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="0e46a-230">Bármely más TeamSeer felhasználói fiók létrehozása eszközök vagy TeamSeer tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="0e46a-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0e46a-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0e46a-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0e46a-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTeamSeer megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0e46a-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTeamSeer.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0e46a-234">**tooassign Britta Simon tooTeamSeer, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0e46a-234">**tooassign Britta Simon tooTeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e46a-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0e46a-237">Hello alkalmazások listában válassza ki a **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-237">In hello applications list, select **TeamSeer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="0e46a-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0e46a-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0e46a-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e46a-241">Click **Add** button.</span></span> <span data-ttu-id="0e46a-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e46a-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0e46a-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0e46a-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0e46a-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e46a-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0e46a-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e46a-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0e46a-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0e46a-247">Testing single sign-on</span></span>

<span data-ttu-id="0e46a-248">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="0e46a-248">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="0e46a-249">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0e46a-249">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e46a-250">További források</span><span class="sxs-lookup"><span data-stu-id="0e46a-250">Additional resources</span></span>

* [<span data-ttu-id="0e46a-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0e46a-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0e46a-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0e46a-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

