---
title: "Oktatóanyag: Azure Active Directoryval integrált TeamSeer |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TeamSeer között."
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
ms.openlocfilehash: 2a5e8f6d1443681c43db95da5cef0b7f2ef92291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="1532d-103">Oktatóanyag: Azure Active Directoryval integrált TeamSeer</span><span class="sxs-lookup"><span data-stu-id="1532d-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="1532d-104">Ebben az oktatóanyagban elsajátíthatja TeamSeer integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1532d-104">In this tutorial, you learn how to integrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1532d-105">TeamSeer integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1532d-105">Integrating TeamSeer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1532d-106">Megadhatja a TeamSeer hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1532d-106">You can control in Azure AD who has access to TeamSeer</span></span>
- <span data-ttu-id="1532d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett TeamSeer (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1532d-107">You can enable your users to automatically get signed-on to TeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1532d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1532d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1532d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1532d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1532d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1532d-110">Prerequisites</span></span>

<span data-ttu-id="1532d-111">Konfigurálása az Azure AD-integrációs TeamSeer, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1532d-111">To configure Azure AD integration with TeamSeer, you need the following items:</span></span>

- <span data-ttu-id="1532d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1532d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1532d-113">Egy TeamSeer egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1532d-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1532d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1532d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1532d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1532d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1532d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1532d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1532d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1532d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1532d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1532d-118">Scenario description</span></span>
<span data-ttu-id="1532d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1532d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1532d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1532d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1532d-121">A gyűjteményből TeamSeer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1532d-121">Adding TeamSeer from the gallery</span></span>
2. <span data-ttu-id="1532d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1532d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-the-gallery"></a><span data-ttu-id="1532d-123">A gyűjteményből TeamSeer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1532d-123">Adding TeamSeer from the gallery</span></span>
<span data-ttu-id="1532d-124">TeamSeer az Azure ad integrálása konfigurálásához szüksége TeamSeer hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="1532d-124">To configure the integration of TeamSeer in to Azure AD, you need to add TeamSeer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1532d-125">**A gyűjteményből TeamSeer hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1532d-125">**To add TeamSeer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1532d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1532d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1532d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1532d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1532d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1532d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1532d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1532d-133">Írja be a keresőmezőbe, **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="1532d-133">In the search box, type **TeamSeer**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="1532d-135">Az eredmények panelen válassza ki a **TeamSeer**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1532d-135">In the results panel, select **TeamSeer**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1532d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1532d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1532d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján TeamSeer</span><span class="sxs-lookup"><span data-stu-id="1532d-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1532d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó TeamSeer a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1532d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TeamSeer is to a user in Azure AD.</span></span> <span data-ttu-id="1532d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a TeamSeer közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1532d-140">In other words, a link relationship between an Azure AD user and the related user in TeamSeer needs to be established.</span></span>

<span data-ttu-id="1532d-141">TeamSeer, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1532d-141">In TeamSeer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1532d-142">Az Azure AD egyszeri bejelentkezést a TeamSeer tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1532d-142">To configure and test Azure AD single sign-on with TeamSeer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1532d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1532d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1532d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1532d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1532d-145">**[TeamSeer tesztfelhasználó létrehozása](#creating-a-teamseer-test-user)**  - való Britta Simon valami TeamSeer, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1532d-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - to have a counterpart of Britta Simon in TeamSeer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1532d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1532d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1532d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1532d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1532d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1532d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1532d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az TeamSeer alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1532d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="1532d-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés TeamSeer, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1532d-150">**To configure Azure AD single sign-on with TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="1532d-151">Az Azure portálon a a **TeamSeer** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1532d-151">In the Azure portal, on the **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1532d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1532d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="1532d-155">Az a **TeamSeer tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1532d-155">On the **TeamSeer Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="1532d-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="1532d-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1532d-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="1532d-158">The value is not real.</span></span> <span data-ttu-id="1532d-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1532d-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="1532d-160">Ügyfél [TeamSeer ügyfél-támogatási csoport](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="1532d-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) to get the value.</span></span> 
 
4. <span data-ttu-id="1532d-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1532d-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="1532d-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1532d-165">A a **TeamSeer konfigurációs** kattintson **konfigurálása TeamSeer** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1532d-165">On the **TeamSeer Configuration** section, click **Configure TeamSeer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1532d-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1532d-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="1532d-168">Egy másik webes böngészőablakban jelentkezzen be a TeamSeer vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1532d-168">In a different web browser window, log in to your TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="1532d-169">Ugrás a **HR Admin**.</span><span class="sxs-lookup"><span data-stu-id="1532d-169">Go to **HR Admin**.</span></span>
   
    <span data-ttu-id="1532d-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR-rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="1532d-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="1532d-171">Kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="1532d-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="1532d-172">![A telepítő](./media/active-directory-saas-teamseer-tutorial/ic789635.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="1532d-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="1532d-173">Kattintson a **SAML szolgáltató részleteinek beállítása**.</span><span class="sxs-lookup"><span data-stu-id="1532d-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="1532d-174">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="1532d-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="1532d-175">A SAML szolgáltató részleteket tartalmazó területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1532d-175">In the SAML provider details section, perform the following steps:</span></span>
   
    <span data-ttu-id="1532d-176">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="1532d-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="1532d-177">a.</span><span class="sxs-lookup"><span data-stu-id="1532d-177">a.</span></span> <span data-ttu-id="1532d-178">Beillesztés a **egyszeri bejelentkezési URL-címe** az értéket a **URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1532d-178">Paste the **Single Sign-On Service URL** value in to the **URL** textbox.</span></span>
          
    <span data-ttu-id="1532d-179">b.</span><span class="sxs-lookup"><span data-stu-id="1532d-179">b.</span></span> <span data-ttu-id="1532d-180">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, meg a tartalom másolása a vágólapra és illessze be azt a **IdP nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1532d-180">Open your base-64 encoded certificate in notepad, copy the content of it in to your clipboard, and then paste it to the **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="1532d-181">A SAML-szolgáltató konfigurációjának befejezéséhez hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1532d-181">To complete the SAML provider configuration, perform the following steps:</span></span>
    
    <span data-ttu-id="1532d-182">![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="1532d-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="1532d-183">a.</span><span class="sxs-lookup"><span data-stu-id="1532d-183">a.</span></span> <span data-ttu-id="1532d-184">Az a **teszt E-mail címek**, írja be a tesztfelhasználó számára e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="1532d-184">In the **Test Email Addresses**, type the test user’s email address.</span></span> 
  
    <span data-ttu-id="1532d-185">b.</span><span class="sxs-lookup"><span data-stu-id="1532d-185">b.</span></span> <span data-ttu-id="1532d-186">Az a **kibocsátó** szövegmező, a szolgáltató kibocsátó URL-címét.</span><span class="sxs-lookup"><span data-stu-id="1532d-186">In the **Issuer** textbox, type the Issuer URL of the service provider.</span></span> 
  
    <span data-ttu-id="1532d-187">c.</span><span class="sxs-lookup"><span data-stu-id="1532d-187">c.</span></span> <span data-ttu-id="1532d-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1532d-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1532d-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1532d-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1532d-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1532d-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1532d-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1532d-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1532d-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="1532d-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1532d-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1532d-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1532d-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1532d-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1532d-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1532d-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1532d-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1532d-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1532d-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1532d-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1532d-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1532d-204">a.</span><span class="sxs-lookup"><span data-stu-id="1532d-204">a.</span></span> <span data-ttu-id="1532d-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1532d-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1532d-206">b.</span><span class="sxs-lookup"><span data-stu-id="1532d-206">b.</span></span> <span data-ttu-id="1532d-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1532d-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1532d-208">c.</span><span class="sxs-lookup"><span data-stu-id="1532d-208">c.</span></span> <span data-ttu-id="1532d-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1532d-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1532d-210">d.</span><span class="sxs-lookup"><span data-stu-id="1532d-210">d.</span></span> <span data-ttu-id="1532d-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="1532d-212">TeamSeer tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1532d-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="1532d-213">Ahhoz, hogy az Azure AD-felhasználók TeamSeer bejelentkezni, akkor ki kell építenie a ShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="1532d-213">To enable Azure AD users to log in to TeamSeer, they must be provisioned in to ShiftPlanning.</span></span> <span data-ttu-id="1532d-214">TeamSeer, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1532d-214">In the case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="1532d-215">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1532d-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="1532d-216">Jelentkezzen be a **TeamSeer** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1532d-216">Log in to your **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="1532d-217">Hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1532d-217">Perform the following steps:</span></span>
   
    <span data-ttu-id="1532d-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR-rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="1532d-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="1532d-219">a.</span><span class="sxs-lookup"><span data-stu-id="1532d-219">a.</span></span> <span data-ttu-id="1532d-220">Ugrás a **HR Admin \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="1532d-220">Go to **HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="1532d-221">b.</span><span class="sxs-lookup"><span data-stu-id="1532d-221">b.</span></span> <span data-ttu-id="1532d-222">Kattintson a **az új felhasználó varázsló futtatása**.</span><span class="sxs-lookup"><span data-stu-id="1532d-222">Click **Run the New User wizard**.</span></span>

3. <span data-ttu-id="1532d-223">Az a **felhasználó adatait** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1532d-223">In the **User Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="1532d-224">![Felhasználó adatait](./media/active-directory-saas-teamseer-tutorial/ic789641.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="1532d-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="1532d-225">a.</span><span class="sxs-lookup"><span data-stu-id="1532d-225">a.</span></span> <span data-ttu-id="1532d-226">Típus a **Utónév**, **vezetékneve**, **felhasználónevet (E-mail címet)** a egy érvényes ki kívánja építeni a a kapcsolódó szövegmezők az AAD-fiókba.</span><span class="sxs-lookup"><span data-stu-id="1532d-226">Type the **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want to provision in to the related textboxes.</span></span>
  
    <span data-ttu-id="1532d-227">b.</span><span class="sxs-lookup"><span data-stu-id="1532d-227">b.</span></span> <span data-ttu-id="1532d-228">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-228">Click **Next**.</span></span>

4. <span data-ttu-id="1532d-229">Kövesse a képernyőn megjelenő utasításokat az új felhasználót, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="1532d-229">Follow the on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="1532d-230">Bármely más TeamSeer felhasználói fiók létrehozása eszközök vagy TeamSeer kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="1532d-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1532d-231">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1532d-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1532d-232">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TeamSeer Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1532d-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TeamSeer.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1532d-234">**Britta Simon hozzárendelése TeamSeer, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1532d-234">**To assign Britta Simon to TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="1532d-235">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1532d-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1532d-237">Az alkalmazások listában válassza ki a **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="1532d-237">In the applications list, select **TeamSeer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="1532d-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1532d-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1532d-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1532d-241">Click **Add** button.</span></span> <span data-ttu-id="1532d-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1532d-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1532d-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1532d-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1532d-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1532d-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1532d-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1532d-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1532d-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1532d-247">Testing single sign-on</span></span>

<span data-ttu-id="1532d-248">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="1532d-248">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="1532d-249">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1532d-249">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1532d-250">További források</span><span class="sxs-lookup"><span data-stu-id="1532d-250">Additional resources</span></span>

* [<span data-ttu-id="1532d-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1532d-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1532d-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1532d-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

