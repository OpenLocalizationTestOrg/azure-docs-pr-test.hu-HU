---
title: "Oktatóanyag: Azure Active Directoryval integrált PlanMyLeave |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és PlanMyLeave között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: ba418a641b339a0d94a3c7b2596d37fbd88a30c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="cebd1-103">Oktatóanyag: Azure Active Directoryval integrált PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="cebd1-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="cebd1-104">Ebben az oktatóanyagban elsajátíthatja PlanMyLeave integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cebd1-104">In this tutorial, you learn how to integrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cebd1-105">PlanMyLeave integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="cebd1-105">Integrating PlanMyLeave with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cebd1-106">Megadhatja a PlanMyLeave hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cebd1-106">You can control in Azure AD who has access to PlanMyLeave</span></span>
- <span data-ttu-id="cebd1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett PlanMyLeave (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cebd1-107">You can enable your users to automatically get signed-on to PlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cebd1-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="cebd1-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="cebd1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cebd1-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cebd1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cebd1-110">Prerequisites</span></span>

<span data-ttu-id="cebd1-111">Konfigurálása az Azure AD-integrációs PlanMyLeave, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="cebd1-111">To configure Azure AD integration with PlanMyLeave, you need the following items:</span></span>

- <span data-ttu-id="cebd1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cebd1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cebd1-113">Egy PlanMyLeave egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="cebd1-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="cebd1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="cebd1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="cebd1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="cebd1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cebd1-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cebd1-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cebd1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cebd1-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="cebd1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cebd1-118">Scenario description</span></span>
<span data-ttu-id="cebd1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cebd1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cebd1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cebd1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cebd1-121">A gyűjteményből PlanMyLeave hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cebd1-121">Adding PlanMyLeave from the gallery</span></span>
2. <span data-ttu-id="cebd1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cebd1-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-the-gallery"></a><span data-ttu-id="cebd1-123">A gyűjteményből PlanMyLeave hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cebd1-123">Adding PlanMyLeave from the gallery</span></span>
<span data-ttu-id="cebd1-124">Az Azure AD integrálása a PlanMyLeave konfigurálásához kell hozzáadnia PlanMyLeave a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="cebd1-124">To configure the integration of PlanMyLeave into Azure AD, you need to add PlanMyLeave from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cebd1-125">**A gyűjteményből PlanMyLeave hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebd1-125">**To add PlanMyLeave from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cebd1-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cebd1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cebd1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cebd1-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="cebd1-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cebd1-133">Írja be a keresőmezőbe, **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-133">In the search box, type **PlanMyLeave**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="cebd1-135">Az eredmények panelen válassza ki a **PlanMyLeave**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cebd1-135">In the results panel, select **PlanMyLeave**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cebd1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cebd1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cebd1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="cebd1-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cebd1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó PlanMyLeave a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="cebd1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PlanMyLeave is to a user in Azure AD.</span></span> <span data-ttu-id="cebd1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a PlanMyLeave közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cebd1-140">In other words, a link relationship between an Azure AD user and the related user in PlanMyLeave needs to be established.</span></span>

<span data-ttu-id="cebd1-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** PlanMyLeave a.</span><span class="sxs-lookup"><span data-stu-id="cebd1-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="cebd1-142">Az Azure AD egyszeri bejelentkezést a PlanMyLeave tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="cebd1-142">To configure and test Azure AD single sign-on with PlanMyLeave, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cebd1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="cebd1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cebd1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="cebd1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cebd1-145">**[PlanMyLeave tesztfelhasználó létrehozása](#creating-a-planmyleave-test-user)**  - való Britta Simon valami PlanMyLeave, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="cebd1-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - to have a counterpart of Britta Simon in PlanMyLeave that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="cebd1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cebd1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cebd1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cebd1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cebd1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cebd1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cebd1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az PlanMyLeave alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cebd1-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="cebd1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés PlanMyLeave, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebd1-150">**To configure Azure AD single sign-on with PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="cebd1-151">Az Azure felügyeleti portálján a a **PlanMyLeave** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-151">In the Azure Management portal, on the **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cebd1-153">A a **egyszeri bejelentkezés** párbeszédpanel lapon, **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="cebd1-153">On the **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="cebd1-155">Az a **PlanMyLeave tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cebd1-155">On the **PlanMyLeave Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="cebd1-157">a.</span><span class="sxs-lookup"><span data-stu-id="cebd1-157">a.</span></span> <span data-ttu-id="cebd1-158">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="cebd1-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="cebd1-159">b.</span><span class="sxs-lookup"><span data-stu-id="cebd1-159">b.</span></span> <span data-ttu-id="cebd1-160">Az a **azonosítóját** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="cebd1-160">In the **Identifer** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cebd1-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="cebd1-161">Please note that these are not the real values.</span></span> <span data-ttu-id="cebd1-162">Akkor frissítheti ezeket az értékeket, és a tényleges URL-cím bejelentkezési azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cebd1-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="cebd1-163">Ügyfél [PlanMyLeave támogatási csoport](mailto:support@planmyleave.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cebd1-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) to get these values.</span></span>

4. <span data-ttu-id="cebd1-164">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-164">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="cebd1-166">A a **új tanúsítvány létrehozása** párbeszédpanel, kattintson a naptár ikonra, és válasszon egy **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-166">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="cebd1-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-167">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="cebd1-169">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-169">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="cebd1-171">Az előugró **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-171">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cebd1-173">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cebd1-173">On the **SAML Signing Certificate** section, click **Certificate (base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="cebd1-175">A a **PlanMyLeave konfigurációs** kattintson **konfigurálása PlanMyLeave** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="cebd1-175">On the **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** to open **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="cebd1-178">Egy másik webes böngészőablakban bejelentkezni a PlanMyLeave Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="cebd1-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="cebd1-179">Ugrás a **rendszerbeállítás**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-179">Go to **System Setup**.</span></span> <span data-ttu-id="cebd1-180">Végül a a **biztonságkezelés** szakaszban kattintson **vállalati SAML beállítások** .</span><span class="sxs-lookup"><span data-stu-id="cebd1-180">Then on the **Security Management** section click **Company SAML settings** .</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="cebd1-182">Az a **SAML beállítások** területen kattintson a szerkesztőben ikonra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-182">On the **SAML Settings** section, click editor icon.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="cebd1-184">Az a **SAML beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cebd1-184">On the **Update SAML Settings** section, perform the following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="cebd1-186">a.</span><span class="sxs-lookup"><span data-stu-id="cebd1-186">a.</span></span>  <span data-ttu-id="cebd1-187">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="cebd1-187">In the **Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="cebd1-188">b.</span><span class="sxs-lookup"><span data-stu-id="cebd1-188">b.</span></span>  <span data-ttu-id="cebd1-189">Nyissa meg a Jegyzettömbben a letöltött fájl tartalmának másolása a csak közötti---megkezdéséhez tanúsítvány---és---vége tanúsítványát---azt a vágólapra, majd illessze be azt a **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cebd1-189">Open your downloaded certificate file in notepad, copy only the content between the ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it to the **Certificate** textbox.</span></span>

    <span data-ttu-id="cebd1-190">c.</span><span class="sxs-lookup"><span data-stu-id="cebd1-190">c.</span></span> <span data-ttu-id="cebd1-191">Állítsa be "**engedélyezése van**"záró"**Igen**".</span><span class="sxs-lookup"><span data-stu-id="cebd1-191">Set "**Is Enable**" to "**Yes**".</span></span>

    <span data-ttu-id="cebd1-192">d.</span><span class="sxs-lookup"><span data-stu-id="cebd1-192">d.</span></span> <span data-ttu-id="cebd1-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cebd1-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cebd1-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="cebd1-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="cebd1-195">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cebd1-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebd1-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cebd1-198">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-198">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cebd1-200">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cebd1-200">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cebd1-202">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebd1-202">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cebd1-204">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cebd1-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cebd1-206">a.</span><span class="sxs-lookup"><span data-stu-id="cebd1-206">a.</span></span> <span data-ttu-id="cebd1-207">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cebd1-208">b.</span><span class="sxs-lookup"><span data-stu-id="cebd1-208">b.</span></span> <span data-ttu-id="cebd1-209">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cebd1-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cebd1-210">c.</span><span class="sxs-lookup"><span data-stu-id="cebd1-210">c.</span></span> <span data-ttu-id="cebd1-211">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cebd1-212">d.</span><span class="sxs-lookup"><span data-stu-id="cebd1-212">d.</span></span> <span data-ttu-id="cebd1-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="cebd1-214">PlanMyLeave tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cebd1-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="cebd1-215">Ez a szakasz célja PlanMyLeave Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cebd1-215">The objective of this section is to create a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="cebd1-216">PlanMyLeave támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="cebd1-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="cebd1-217">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="cebd1-217">There is no action item for you in this section.</span></span> <span data-ttu-id="cebd1-218">Új felhasználó jön létre az PlanMyLeave elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="cebd1-218">A new user will be created during an attempt to access PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="cebd1-219">Ha hozzon létre manuálisan egy felhasználó van szüksége, forduljon a kell [PlanMyLeave támogatási csoport](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="cebd1-219">If you need to create an user manually, you need to contact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cebd1-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cebd1-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cebd1-221">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés PlanMyLeave Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="cebd1-221">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to PlanMyLeave.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cebd1-223">**Britta Simon hozzárendelése PlanMyLeave, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebd1-223">**To assign Britta Simon to PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="cebd1-224">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-224">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cebd1-226">Az alkalmazások listában válassza ki a **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-226">In the applications list, select **PlanMyLeave**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="cebd1-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cebd1-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cebd1-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cebd1-230">Click **Add** button.</span></span> <span data-ttu-id="cebd1-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebd1-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cebd1-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cebd1-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cebd1-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebd1-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cebd1-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebd1-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="cebd1-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cebd1-236">Testing single sign-on</span></span>

<span data-ttu-id="cebd1-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cebd1-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cebd1-238">Ha a hozzáférési panelen PlanMyLeave csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az PlanMyLeave alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="cebd1-238">When you click the PlanMyLeave tile in the Access Panel, you should get automatically signed-on to your PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cebd1-239">További források</span><span class="sxs-lookup"><span data-stu-id="cebd1-239">Additional resources</span></span>

* [<span data-ttu-id="cebd1-240">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="cebd1-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cebd1-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cebd1-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png