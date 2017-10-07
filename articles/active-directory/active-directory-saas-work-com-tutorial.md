---
title: "Oktatóanyag: Azure Active Directoryval integrált Work.com |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Work.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="75983-103">Oktatóanyag: Azure Active Directoryval integrált Work.com</span><span class="sxs-lookup"><span data-stu-id="75983-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="75983-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Work.com az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="75983-104">In this tutorial, you learn how toointegrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75983-105">Work.com integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="75983-105">Integrating Work.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75983-106">Megadhatja a hozzáférés tooWork.com rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="75983-106">You can control in Azure AD who has access tooWork.com</span></span>
- <span data-ttu-id="75983-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWork.com (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="75983-107">You can enable your users tooautomatically get signed-on tooWork.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75983-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="75983-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="75983-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75983-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75983-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75983-110">Prerequisites</span></span>

<span data-ttu-id="75983-111">az Azure AD integrálása Work.com tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="75983-111">tooconfigure Azure AD integration with Work.com, you need hello following items:</span></span>

- <span data-ttu-id="75983-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="75983-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75983-113">Egy Work.com egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="75983-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75983-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="75983-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75983-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="75983-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75983-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="75983-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75983-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75983-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75983-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="75983-118">Scenario description</span></span>
<span data-ttu-id="75983-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="75983-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75983-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="75983-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75983-121">Adja hozzá a Work.com hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="75983-121">Add Work.com from hello gallery</span></span>
2. <span data-ttu-id="75983-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75983-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-hello-gallery"></a><span data-ttu-id="75983-123">Adja hozzá a Work.com hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="75983-123">Add Work.com from hello gallery</span></span>
<span data-ttu-id="75983-124">tooconfigure hello integrációja Work.com az Azure AD-be, meg kell tooadd Work.com hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="75983-124">tooconfigure hello integration of Work.com into Azure AD, you need tooadd Work.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75983-125">**tooadd Work.com hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="75983-125">**tooadd Work.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75983-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75983-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75983-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="75983-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75983-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75983-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="75983-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="75983-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="75983-133">Hello keresési mezőbe, írja be a **Work.com**, jelölje be **Work.com** eredmények panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-133">In hello search box, type **Work.com**, select **Work.com** from results panel then click **Add** button tooadd hello application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="75983-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75983-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="75983-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Work.com.</span><span class="sxs-lookup"><span data-stu-id="75983-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="75983-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Work.com tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="75983-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Work.com is tooa user in Azure AD.</span></span> <span data-ttu-id="75983-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Work.com közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="75983-138">In other words, a link relationship between an Azure AD user and hello related user in Work.com needs toobe established.</span></span>

<span data-ttu-id="75983-139">Work.com, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="75983-139">In Work.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="75983-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Work.com-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="75983-140">tooconfigure and test Azure AD single sign-on with Work.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75983-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="75983-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75983-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75983-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75983-143">**[Work.com tesztfelhasználó létrehozása](#create-a-workcom-test-user)**  -toohave egy megfelelője a Britta Simon a Work.com, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="75983-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - toohave a counterpart of Britta Simon in Work.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="75983-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="75983-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75983-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="75983-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="75983-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75983-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="75983-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Work.com alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="75983-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="75983-148">tooconfigure egyszeri bejelentkezést, kell toosetup egy Work.com egyéni tartománynév még.</span><span class="sxs-lookup"><span data-stu-id="75983-148">tooconfigure single sign-on, you need toosetup a custom Work.com domain name yet.</span></span> <span data-ttu-id="75983-149">Toodefine legalább egy tartományhoz kell neve, a tartománynév tesztelése és telepítheti azt tooyour teljes szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="75983-149">You need toodefine at least a domain name, test your domain name, and deploy it tooyour entire organization.</span></span>

<span data-ttu-id="75983-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Work.com, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="75983-150">**tooconfigure Azure AD single sign-on with Work.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="75983-151">Az Azure portál, a hello hello **Work.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="75983-151">In hello Azure portal, on hello **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="75983-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="75983-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="75983-155">A hello **Work.com tartomány és az URL-címek** területen hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="75983-155">On hello **Work.com Domain and URLs** section, perform hello following:</span></span>

    ![Work.com tartomány és az URL-címek szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="75983-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="75983-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="75983-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="75983-158">This value is not real.</span></span> <span data-ttu-id="75983-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="75983-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="75983-160">Ügyfél [Work.com ügyfél-támogatási csoport](https://help.salesforce.com/articleView?id=000159855&type=3) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="75983-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) tooget this value.</span></span> 

4. <span data-ttu-id="75983-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="75983-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="75983-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-163">Click **Save** button.</span></span>

    ![Mentés gomb](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75983-165">A hello **Work.com konfigurációs** kattintson **konfigurálása Work.com** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="75983-165">On hello **Work.com Configuration** section, click **Configure Work.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="75983-166">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="75983-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Work.com konfigurációs szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="75983-168">Jelentkezzen be tooyour Work.com Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="75983-168">Log in tooyour Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="75983-169">Nyissa meg túl**telepítő**.</span><span class="sxs-lookup"><span data-stu-id="75983-169">Go too**Setup**.</span></span>
   
    <span data-ttu-id="75983-170">![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="75983-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="75983-171">A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap.</span><span class="sxs-lookup"><span data-stu-id="75983-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
    <span data-ttu-id="75983-172">![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic767825.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="75983-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="75983-173">amely a tartomány megfelelően be van állítva tooverify győződjön meg arról, hogy "**lépés 4 telepített tooUsers**", és tekintse át a "**saját tartománybeállítások**".</span><span class="sxs-lookup"><span data-stu-id="75983-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="75983-174">![Tartományban üzembe helyezett tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "tartományban telepített tooUser")</span><span class="sxs-lookup"><span data-stu-id="75983-174">![Domain Deployed tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="75983-175">Jelentkezzen be tooyour Work.com bérlő.</span><span class="sxs-lookup"><span data-stu-id="75983-175">Log in tooyour Work.com tenant.</span></span>

12. <span data-ttu-id="75983-176">Nyissa meg túl**telepítő**.</span><span class="sxs-lookup"><span data-stu-id="75983-176">Go too**Setup**.</span></span>
    
    <span data-ttu-id="75983-177">![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="75983-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="75983-178">Bontsa ki a hello **biztonsági vezérlők** menüben, majd kattintson **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="75983-178">Expand hello **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="75983-179">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-work-com-tutorial/ic794113.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="75983-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="75983-180">A hello **egyszeri bejelentkezési beállítások** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="75983-180">On hello **Single Sign-On Settings** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="75983-181">![A SAML engedélyezett](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML engedélyezett")</span><span class="sxs-lookup"><span data-stu-id="75983-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="75983-182">a.</span><span class="sxs-lookup"><span data-stu-id="75983-182">a.</span></span> <span data-ttu-id="75983-183">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="75983-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="75983-184">b.</span><span class="sxs-lookup"><span data-stu-id="75983-184">b.</span></span> <span data-ttu-id="75983-185">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="75983-185">Click **New**.</span></span>

15. <span data-ttu-id="75983-186">A hello **SAML-alapú egyszeri bejelentkezés beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="75983-186">In hello **SAML Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="75983-187">![SAML-alapú egyszeri bejelentkezés beállítása](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML-alapú egyszeri bejelentkezés beállítása")</span><span class="sxs-lookup"><span data-stu-id="75983-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="75983-188">a.</span><span class="sxs-lookup"><span data-stu-id="75983-188">a.</span></span> <span data-ttu-id="75983-189">A hello **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="75983-189">In hello **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="75983-190">Értéket biztosító **neve** automatikusan feltölti a hello **API-név** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="75983-190">Providing a value for **Name** does automatically populate hello **API Name** textbox.</span></span>
    
    <span data-ttu-id="75983-191">b.</span><span class="sxs-lookup"><span data-stu-id="75983-191">b.</span></span> <span data-ttu-id="75983-192">A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="75983-192">In **Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="75983-193">c.</span><span class="sxs-lookup"><span data-stu-id="75983-193">c.</span></span> <span data-ttu-id="75983-194">az Azure-portálon tooupload letöltött hello tanúsítványát kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="75983-194">tooupload hello downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="75983-195">d.</span><span class="sxs-lookup"><span data-stu-id="75983-195">d.</span></span> <span data-ttu-id="75983-196">A hello **entitásazonosító** szövegmezőhöz típus `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="75983-196">In hello **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="75983-197">e.</span><span class="sxs-lookup"><span data-stu-id="75983-197">e.</span></span> <span data-ttu-id="75983-198">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.</span><span class="sxs-lookup"><span data-stu-id="75983-198">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
    
    <span data-ttu-id="75983-199">f.</span><span class="sxs-lookup"><span data-stu-id="75983-199">f.</span></span> <span data-ttu-id="75983-200">Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentfier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="75983-200">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>
    
    <span data-ttu-id="75983-201">g.</span><span class="sxs-lookup"><span data-stu-id="75983-201">g.</span></span> <span data-ttu-id="75983-202">A **Identity Provider bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="75983-202">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="75983-203">h.</span><span class="sxs-lookup"><span data-stu-id="75983-203">h.</span></span> <span data-ttu-id="75983-204">A **Identity Provider kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="75983-204">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="75983-205">i.</span><span class="sxs-lookup"><span data-stu-id="75983-205">i.</span></span> <span data-ttu-id="75983-206">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="75983-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="75983-207">j.</span><span class="sxs-lookup"><span data-stu-id="75983-207">j.</span></span> <span data-ttu-id="75983-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-208">Click **Save**.</span></span>

16. <span data-ttu-id="75983-209">A Work.com klasszikus portál, a hello bal oldali navigációs panelen, kattintson a **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello **saját tartomány**lap.</span><span class="sxs-lookup"><span data-stu-id="75983-209">In your Work.com classic portal, on hello left navigation pane, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="75983-210">![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic794115.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="75983-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="75983-211">A hello **saját tartomány** lap hello **bejelentkezési oldal vállalati arculata** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="75983-211">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="75983-212">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic767826.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="75983-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="75983-213">A hello **bejelentkezési oldal vállalati arculata** lap hello **hitelesítési szolgáltatás** szakaszban, hello nevét a **SAML SSO beállítások** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="75983-213">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="75983-214">Válassza ki azt, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="75983-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="75983-215">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic784366.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="75983-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="75983-216">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="75983-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="75983-217">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="75983-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="75983-218">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75983-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="75983-219">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="75983-219">Create an Azure AD test user</span></span>
<span data-ttu-id="75983-220">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="75983-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="75983-222">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="75983-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75983-223">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75983-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75983-225">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="75983-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75983-227">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75983-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Hozzáadás](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75983-229">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="75983-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75983-231">a.</span><span class="sxs-lookup"><span data-stu-id="75983-231">a.</span></span> <span data-ttu-id="75983-232">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75983-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75983-233">b.</span><span class="sxs-lookup"><span data-stu-id="75983-233">b.</span></span> <span data-ttu-id="75983-234">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75983-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75983-235">c.</span><span class="sxs-lookup"><span data-stu-id="75983-235">c.</span></span> <span data-ttu-id="75983-236">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="75983-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="75983-237">d.</span><span class="sxs-lookup"><span data-stu-id="75983-237">d.</span></span> <span data-ttu-id="75983-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="75983-239">Work.com tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="75983-239">Create a Work.com test user</span></span>
<span data-ttu-id="75983-240">Az Azure Active Directory felhasználók toobe képes toosign a kiosztott tooWork.com kell lenniük. Work.com hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="75983-240">For Azure Active Directory users toobe able toosign in, they must be provisioned tooWork.com. In hello case of Work.com, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="75983-241">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="75983-241">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="75983-242">Bejelentkezés tooyour Work.com vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="75983-242">Sign on tooyour Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="75983-243">Nyissa meg túl**telepítő**.</span><span class="sxs-lookup"><span data-stu-id="75983-243">Go too**Setup**.</span></span>
   
    <span data-ttu-id="75983-244">![A telepítő](./media/active-directory-saas-work-com-tutorial/IC794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="75983-244">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="75983-245">Nyissa meg túl**felhasználók kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="75983-245">Go too**Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="75983-246">![Felhasználók kezelése](./media/active-directory-saas-work-com-tutorial/IC784369.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="75983-246">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="75983-247">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="75983-247">Click **New User**.</span></span>
   
    <span data-ttu-id="75983-248">![Minden felhasználó](./media/active-directory-saas-work-com-tutorial/IC794117.png "minden felhasználó")</span><span class="sxs-lookup"><span data-stu-id="75983-248">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="75983-249">Hello szakasz felhasználó szerkesztése, hajtsa végre a következő lépéseket a egy érvényes Azure attribútumait hello tooprovision a kívánt hello AD fiókhoz kapcsolódó szövegmezők:</span><span class="sxs-lookup"><span data-stu-id="75983-249">In hello User Edit section, perform hello following steps, in attributes of a valid Azure AD account you want tooprovision into hello related textboxes:</span></span>
   
    <span data-ttu-id="75983-250">![Felhasználó szerkesztése](./media/active-directory-saas-work-com-tutorial/ic794118.png "felhasználó szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="75983-250">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="75983-251">a.</span><span class="sxs-lookup"><span data-stu-id="75983-251">a.</span></span> <span data-ttu-id="75983-252">A hello **Utónév** szövegmezőhöz típus hello **Utónév** hello felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="75983-252">In hello **First Name** textbox, type hello **first name** of hello user **Britta**.</span></span>
    
    <span data-ttu-id="75983-253">b.</span><span class="sxs-lookup"><span data-stu-id="75983-253">b.</span></span> <span data-ttu-id="75983-254">A hello **Vezetéknév** szövegmezőhöz típus hello **Vezetéknév** hello felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="75983-254">In hello **Last Name** textbox, type hello **last name** of hello user **Simon**.</span></span>
    
    <span data-ttu-id="75983-255">c.</span><span class="sxs-lookup"><span data-stu-id="75983-255">c.</span></span> <span data-ttu-id="75983-256">A hello **Alias** szövegmezőhöz típus hello **neve** hello felhasználó **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="75983-256">In hello **Alias** textbox, type hello **name** of hello user **BrittaS**.</span></span>
    
    <span data-ttu-id="75983-257">d.</span><span class="sxs-lookup"><span data-stu-id="75983-257">d.</span></span> <span data-ttu-id="75983-258">A hello **E-mail** szövegmezőhöz típus hello **e-mail cím** felhasználó  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="75983-258">In hello **Email** textbox, type hello **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="75983-259">e.</span><span class="sxs-lookup"><span data-stu-id="75983-259">e.</span></span> <span data-ttu-id="75983-260">A hello **felhasználónév** szövegmező, írja be például a felhasználó a felhasználónév  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="75983-260">In hello **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="75983-261">f.</span><span class="sxs-lookup"><span data-stu-id="75983-261">f.</span></span> <span data-ttu-id="75983-262">A hello **becenév** szövegmező, adjon meg egy **becenév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="75983-262">In hello **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="75983-263">g.</span><span class="sxs-lookup"><span data-stu-id="75983-263">g.</span></span> <span data-ttu-id="75983-264">Válassza ki **szerepkör**, **felhasználói licenc**, és **profil**.</span><span class="sxs-lookup"><span data-stu-id="75983-264">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="75983-265">h.</span><span class="sxs-lookup"><span data-stu-id="75983-265">h.</span></span> <span data-ttu-id="75983-266">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-266">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="75983-267">az Azure AD fióktulajdonos hello kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="75983-267">hello Azure AD account holder will get an email including a link tooconfirm hello account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="75983-268">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="75983-268">Assign hello Azure AD test user</span></span>

<span data-ttu-id="75983-269">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWork.com megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="75983-269">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWork.com.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="75983-271">**tooassign Britta Simon tooWork.com, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="75983-271">**tooassign Britta Simon tooWork.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="75983-272">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75983-272">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="75983-274">Hello alkalmazások listában válassza ki a **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="75983-274">In hello applications list, select **Work.com**.</span></span>

    ![Alkalmazás listában Work.com](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="75983-276">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="75983-276">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="75983-278">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="75983-278">Click **Add** button.</span></span> <span data-ttu-id="75983-279">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75983-279">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="75983-281">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="75983-281">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75983-282">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75983-282">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75983-283">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75983-283">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="75983-284">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="75983-284">Test single sign-on</span></span>

<span data-ttu-id="75983-285">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="75983-285">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="75983-286">Ha a hozzáférési Panel hello hello Work.com csempe gombra kattint, automatikusan bejelentkezett tooyour Work.com alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="75983-286">When you click hello Work.com tile in hello Access Panel, you should get automatically signed-on tooyour Work.com application.</span></span>
<span data-ttu-id="75983-287">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="75983-287">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75983-288">További források</span><span class="sxs-lookup"><span data-stu-id="75983-288">Additional resources</span></span>

* [<span data-ttu-id="75983-289">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="75983-289">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75983-290">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="75983-290">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

