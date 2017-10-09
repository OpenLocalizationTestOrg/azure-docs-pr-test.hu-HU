---
title: "Oktatóanyag: Azure Active Directoryval integrált TargetProcess |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TargetProcess között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 05c574e2c18d7f73edc6c094093a6e59d46b8e6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="76f15-103">Oktatóanyag: Azure Active Directoryval integrált TargetProcess</span><span class="sxs-lookup"><span data-stu-id="76f15-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="76f15-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TargetProcess az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="76f15-104">In this tutorial, you learn how toointegrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="76f15-105">TargetProcess integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="76f15-105">Integrating TargetProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="76f15-106">Megadhatja a hozzáférés tooTargetProcess rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="76f15-106">You can control in Azure AD who has access tooTargetProcess</span></span>
- <span data-ttu-id="76f15-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTargetProcess (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="76f15-107">You can enable your users tooautomatically get signed-on tooTargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="76f15-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="76f15-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="76f15-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="76f15-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76f15-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="76f15-110">Prerequisites</span></span>

<span data-ttu-id="76f15-111">az Azure AD integrálása TargetProcess tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="76f15-111">tooconfigure Azure AD integration with TargetProcess, you need hello following items:</span></span>

- <span data-ttu-id="76f15-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="76f15-112">An Azure AD subscription</span></span>
- <span data-ttu-id="76f15-113">Egy TargetProcess egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="76f15-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="76f15-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="76f15-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="76f15-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="76f15-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="76f15-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="76f15-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="76f15-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="76f15-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="76f15-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="76f15-118">Scenario description</span></span>
<span data-ttu-id="76f15-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="76f15-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="76f15-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="76f15-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="76f15-121">Adja hozzá a TargetProcess hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="76f15-121">Add TargetProcess from hello gallery</span></span>
2. <span data-ttu-id="76f15-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="76f15-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-hello-gallery"></a><span data-ttu-id="76f15-123">Adja hozzá a TargetProcess hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="76f15-123">Add TargetProcess from hello gallery</span></span>
<span data-ttu-id="76f15-124">tooconfigure hello integrációja TargetProcess az Azure AD-be, meg kell tooadd TargetProcess hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="76f15-124">tooconfigure hello integration of TargetProcess into Azure AD, you need tooadd TargetProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="76f15-125">**tooadd TargetProcess hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="76f15-125">**tooadd TargetProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="76f15-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="76f15-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="76f15-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="76f15-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="76f15-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="76f15-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="76f15-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="76f15-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="76f15-133">Hello keresési mezőbe, írja be a **TargetProcess**, jelölje be **TargetProcess** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="76f15-133">In hello search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hozzáadás TargetProcess gyűjteményből](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="76f15-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="76f15-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="76f15-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="76f15-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="76f15-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TargetProcess tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="76f15-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TargetProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="76f15-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TargetProcess közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="76f15-138">In other words, a link relationship between an Azure AD user and hello related user in TargetProcess needs toobe established.</span></span>

<span data-ttu-id="76f15-139">TargetProcess, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="76f15-139">In TargetProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="76f15-140">tooconfigure és az Azure AD az egyszeri bejelentkezés TargetProcess-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="76f15-140">tooconfigure and test Azure AD single sign-on with TargetProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="76f15-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="76f15-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="76f15-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="76f15-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="76f15-143">**[TargetProcess tesztfelhasználó létrehozása](#create-a-targetprocess-test-user)**  -toohave egy megfelelője a Britta Simon a TargetProcess, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="76f15-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - toohave a counterpart of Britta Simon in TargetProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="76f15-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76f15-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="76f15-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="76f15-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="76f15-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="76f15-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="76f15-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TargetProcess alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="76f15-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="76f15-148">**az Azure AD tooconfigure egyszeri bejelentkezést a TargetProcess, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="76f15-148">**tooconfigure Azure AD single sign-on with TargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="76f15-149">Az Azure portál, a hello hello **TargetProcess** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="76f15-149">In hello Azure portal, on hello **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="76f15-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76f15-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="76f15-153">A hello **TargetProcess tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="76f15-153">On hello **TargetProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![TargetProcess tartomány és az URL-címek szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="76f15-155">a.</span><span class="sxs-lookup"><span data-stu-id="76f15-155">a.</span></span> <span data-ttu-id="76f15-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="76f15-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="76f15-157">b.</span><span class="sxs-lookup"><span data-stu-id="76f15-157">b.</span></span> <span data-ttu-id="76f15-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="76f15-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="76f15-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="76f15-159">These values are not real.</span></span> <span data-ttu-id="76f15-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="76f15-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="76f15-161">Ügyfél [TargetProcess ügyfél-támogatási csoport](mailto:support@targetprocess.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="76f15-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="76f15-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="76f15-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="76f15-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="76f15-164">Click **Save** button.</span></span>

    ![Mentés gombja](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="76f15-166">A hello **TargetProcess konfigurációs** kattintson **konfigurálása TargetProcess** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="76f15-166">On hello **TargetProcess Configuration** section, click **Configure TargetProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="76f15-167">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="76f15-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TargetProcess konfigurációs szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="76f15-169">Bejelentkezés tooyour TargetProcess alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="76f15-169">Sign-on tooyour TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="76f15-170">Hello hello felső menüben kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="76f15-170">In hello menu on hello top, click **Setup**.</span></span>
   
    ![Beállítás](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="76f15-172">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="76f15-172">Click **Settings**.</span></span>
   
    ![Beállítások](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="76f15-174">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="76f15-174">Click **Single Sign-on**.</span></span>
   
    ![Kattintson az egyszeri bejelentkezést.](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="76f15-176">Hello egyszeri bejelentkezés beállítások párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="76f15-176">On hello Single Sign-on settings dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="76f15-178">a.</span><span class="sxs-lookup"><span data-stu-id="76f15-178">a.</span></span> <span data-ttu-id="76f15-179">Kattintson a **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="76f15-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="76f15-180">b.</span><span class="sxs-lookup"><span data-stu-id="76f15-180">b.</span></span> <span data-ttu-id="76f15-181">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="76f15-181">In **Sign-on URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="76f15-182">c.</span><span class="sxs-lookup"><span data-stu-id="76f15-182">c.</span></span> <span data-ttu-id="76f15-183">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="76f15-183">Open your downloaded certificate in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="76f15-184">d.</span><span class="sxs-lookup"><span data-stu-id="76f15-184">d.</span></span> <span data-ttu-id="76f15-185">Kattintson a **JIT-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="76f15-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="76f15-186">e.</span><span class="sxs-lookup"><span data-stu-id="76f15-186">e.</span></span> <span data-ttu-id="76f15-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="76f15-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="76f15-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="76f15-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="76f15-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="76f15-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="76f15-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="76f15-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="76f15-191">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="76f15-191">Create an Azure AD test user</span></span>
<span data-ttu-id="76f15-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="76f15-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="76f15-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="76f15-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="76f15-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="76f15-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="76f15-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="76f15-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![felhasználók toodisplay hello listája](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="76f15-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76f15-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="76f15-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="76f15-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Felhasználói szakasz](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="76f15-203">a.</span><span class="sxs-lookup"><span data-stu-id="76f15-203">a.</span></span> <span data-ttu-id="76f15-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="76f15-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="76f15-205">b.</span><span class="sxs-lookup"><span data-stu-id="76f15-205">b.</span></span> <span data-ttu-id="76f15-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="76f15-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="76f15-207">c.</span><span class="sxs-lookup"><span data-stu-id="76f15-207">c.</span></span> <span data-ttu-id="76f15-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="76f15-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="76f15-209">d.</span><span class="sxs-lookup"><span data-stu-id="76f15-209">d.</span></span> <span data-ttu-id="76f15-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="76f15-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="76f15-211">TargetProcess tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="76f15-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="76f15-212">hello ebben a szakaszban célja toocreate TargetProcess Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="76f15-212">hello objective of this section is toocreate a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="76f15-213">TargetProcess támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="76f15-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="76f15-214">Már engedélyezett legyen [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="76f15-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="76f15-215">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="76f15-215">There is no action item for you in this section.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="76f15-216">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="76f15-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="76f15-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTargetProcess megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="76f15-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTargetProcess.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="76f15-219">**tooassign Britta Simon tooTargetProcess, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="76f15-219">**tooassign Britta Simon tooTargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="76f15-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="76f15-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="76f15-222">Hello alkalmazások listában válassza ki a **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="76f15-222">In hello applications list, select **TargetProcess**.</span></span>

    ![TargetProcess alkalmazáslistában](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="76f15-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="76f15-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="76f15-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="76f15-226">Click **Add** button.</span></span> <span data-ttu-id="76f15-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76f15-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="76f15-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="76f15-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="76f15-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76f15-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="76f15-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76f15-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="76f15-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="76f15-232">Test single sign-on</span></span>

<span data-ttu-id="76f15-233">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="76f15-233">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="76f15-234">Hello TargetProcess hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour TargetProcess alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="76f15-234">When you click hello TargetProcess tile in hello Access Panel, you should get automatically signed-on tooyour TargetProcess application.</span></span> <span data-ttu-id="76f15-235">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="76f15-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76f15-236">További források</span><span class="sxs-lookup"><span data-stu-id="76f15-236">Additional resources</span></span>

* [<span data-ttu-id="76f15-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="76f15-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="76f15-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="76f15-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

