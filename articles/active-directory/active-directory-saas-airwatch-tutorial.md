---
title: "Oktatóanyag: Azure Active Directoryval integrált AirWatch |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és AirWatch között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1996ec97e7c0d94c5606ca43bb5956548f1f3712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="30c46-103">Oktatóanyag: Azure Active Directoryval integrált AirWatch</span><span class="sxs-lookup"><span data-stu-id="30c46-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="30c46-104">Ebben az oktatóanyagban elsajátíthatja AirWatch integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="30c46-104">In this tutorial, you learn how to integrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30c46-105">AirWatch integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="30c46-105">Integrating AirWatch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="30c46-106">Megadhatja a AirWatch hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="30c46-106">You can control in Azure AD who has access to AirWatch</span></span>
- <span data-ttu-id="30c46-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett AirWatch (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="30c46-107">You can enable your users to automatically get signed-on to AirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="30c46-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="30c46-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="30c46-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30c46-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30c46-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30c46-110">Prerequisites</span></span>

<span data-ttu-id="30c46-111">Konfigurálása az Azure AD-integrációs AirWatch, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="30c46-111">To configure Azure AD integration with AirWatch, you need the following items:</span></span>

- <span data-ttu-id="30c46-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="30c46-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30c46-113">Egy AirWatch egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="30c46-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="30c46-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="30c46-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="30c46-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="30c46-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="30c46-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="30c46-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="30c46-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30c46-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30c46-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="30c46-118">Scenario description</span></span>
<span data-ttu-id="30c46-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="30c46-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="30c46-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="30c46-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30c46-121">A gyűjteményből AirWatch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30c46-121">Adding AirWatch from the gallery</span></span>
2. <span data-ttu-id="30c46-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="30c46-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-the-gallery"></a><span data-ttu-id="30c46-123">A gyűjteményből AirWatch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30c46-123">Adding AirWatch from the gallery</span></span>
<span data-ttu-id="30c46-124">Az Azure AD integrálása a AirWatch konfigurálásához kell hozzáadnia AirWatch a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="30c46-124">To configure the integration of AirWatch into Azure AD, you need to add AirWatch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="30c46-125">**A gyűjteményből AirWatch hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="30c46-125">**To add AirWatch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="30c46-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="30c46-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="30c46-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="30c46-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="30c46-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30c46-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="30c46-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="30c46-133">Írja be a keresőmezőbe, **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="30c46-133">In the search box, type **AirWatch**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="30c46-135">Az eredmények panelen válassza ki a **AirWatch**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="30c46-135">In the results panel, select **AirWatch**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="30c46-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="30c46-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="30c46-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AirWatch</span><span class="sxs-lookup"><span data-stu-id="30c46-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="30c46-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó AirWatch a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="30c46-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AirWatch is to a user in Azure AD.</span></span> <span data-ttu-id="30c46-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a AirWatch közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="30c46-140">In other words, a link relationship between an Azure AD user and the related user in AirWatch needs to be established.</span></span>

<span data-ttu-id="30c46-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** AirWatch a.</span><span class="sxs-lookup"><span data-stu-id="30c46-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in AirWatch.</span></span>

<span data-ttu-id="30c46-142">Az Azure AD egyszeri bejelentkezést a AirWatch tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="30c46-142">To configure and test Azure AD single sign-on with AirWatch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="30c46-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="30c46-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="30c46-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="30c46-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="30c46-145">**[AirWatch tesztfelhasználó létrehozása](#creating-a-airwatch-test-user)**  - való Britta Simon valami AirWatch, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="30c46-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - to have a counterpart of Britta Simon in AirWatch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="30c46-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="30c46-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30c46-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="30c46-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="30c46-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30c46-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="30c46-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az AirWatch alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="30c46-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="30c46-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés AirWatch, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="30c46-150">**To configure Azure AD single sign-on with AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="30c46-151">Az Azure portálon a a **AirWatch** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="30c46-151">In the Azure portal, on the **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="30c46-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="30c46-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="30c46-155">Az a **AirWatch tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="30c46-155">On the **AirWatch Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="30c46-157">a.</span><span class="sxs-lookup"><span data-stu-id="30c46-157">a.</span></span> <span data-ttu-id="30c46-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="30c46-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="30c46-159">b.</span><span class="sxs-lookup"><span data-stu-id="30c46-159">b.</span></span> <span data-ttu-id="30c46-160">Az a **azonosító** szövegmező, írja be az értéket, mint`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="30c46-160">In the **Identifier** textbox, type the value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="30c46-161">Ez az érték nem a valós.</span><span class="sxs-lookup"><span data-stu-id="30c46-161">This value is not the real.</span></span> <span data-ttu-id="30c46-162">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="30c46-162">Update this value with the actual Sign-on URL.</span></span> <span data-ttu-id="30c46-163">Ügyfél [AirWatch ügyfél-támogatási csoport](http://www.air-watch.com/company/contact-us/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="30c46-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) to get this value.</span></span> 
 
4. <span data-ttu-id="30c46-164">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="30c46-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="30c46-166">A a **AirWatch konfigurációs** kattintson **konfigurálása AirWatch** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="30c46-166">On the **AirWatch Configuration** section, click **Configure AirWatch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="30c46-167">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="30c46-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="30c46-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-169">Click **Save** button.</span></span>

    <span data-ttu-id="30c46-170">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="30c46-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="30c46-171">Egy másik webes böngészőablakban jelentkezzen be a AirWatch vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="30c46-171">In a different web browser window, log in to your AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="30c46-172">A bal oldali navigációs ablaktáblán kattintson **fiókok**, és kattintson a **rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="30c46-172">In the left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="30c46-173">![A rendszergazdák](./media/active-directory-saas-airwatch-tutorial/ic791920.png "rendszergazdák")</span><span class="sxs-lookup"><span data-stu-id="30c46-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="30c46-174">Bontsa ki a **beállítások** menüben, majd kattintson **címtárszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="30c46-174">Expand the **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="30c46-175">![Beállítások](./media/active-directory-saas-airwatch-tutorial/ic791921.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="30c46-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="30c46-176">Kattintson a **felhasználói** lap a **Base DN** szövegmező, írja be a tartomány nevét, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="30c46-176">Click the **User** tab, in the **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="30c46-177">![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791922.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="30c46-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="30c46-178">Kattintson a **Server** fülre.</span><span class="sxs-lookup"><span data-stu-id="30c46-178">Click the **Server** tab.</span></span>
   
   <span data-ttu-id="30c46-179">![Kiszolgáló](./media/active-directory-saas-airwatch-tutorial/ic791923.png "kiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="30c46-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="30c46-180">Hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="30c46-180">Perform the following steps:</span></span>
    
    <span data-ttu-id="30c46-181">![Töltse fel](./media/active-directory-saas-airwatch-tutorial/ic791924.png "feltöltése")</span><span class="sxs-lookup"><span data-stu-id="30c46-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="30c46-182">a.</span><span class="sxs-lookup"><span data-stu-id="30c46-182">a.</span></span> <span data-ttu-id="30c46-183">Mint **Directory típusa**, jelölje be **nincs**.</span><span class="sxs-lookup"><span data-stu-id="30c46-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="30c46-184">b.</span><span class="sxs-lookup"><span data-stu-id="30c46-184">b.</span></span> <span data-ttu-id="30c46-185">Válassza ki **SAML-alapú hitelesítéshez használandó**.</span><span class="sxs-lookup"><span data-stu-id="30c46-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="30c46-186">c.</span><span class="sxs-lookup"><span data-stu-id="30c46-186">c.</span></span> <span data-ttu-id="30c46-187">A letöltött tanúsítvány feltöltése, kattintson a **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="30c46-187">To upload the downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="30c46-188">Az a **kérelem** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="30c46-188">In the **Request** section, perform the following steps:</span></span>
    
    <span data-ttu-id="30c46-189">![Kérelem](./media/active-directory-saas-airwatch-tutorial/ic791925.png "kérése")</span><span class="sxs-lookup"><span data-stu-id="30c46-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="30c46-190">a.</span><span class="sxs-lookup"><span data-stu-id="30c46-190">a.</span></span> <span data-ttu-id="30c46-191">Mint **kötési típus kérése**, jelölje be **POST**.</span><span class="sxs-lookup"><span data-stu-id="30c46-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="30c46-192">b.</span><span class="sxs-lookup"><span data-stu-id="30c46-192">b.</span></span> <span data-ttu-id="30c46-193">Az Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Airwatch** párbeszédpanel lap, a Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be azt a **Identity Provider egyszeri bejelentkezés URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="30c46-193">In the Azure portal, on the **Configure single sign-on at Airwatch** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="30c46-194">c.</span><span class="sxs-lookup"><span data-stu-id="30c46-194">c.</span></span> <span data-ttu-id="30c46-195">Mint **NameID formátum**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="30c46-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="30c46-196">d.</span><span class="sxs-lookup"><span data-stu-id="30c46-196">d.</span></span> <span data-ttu-id="30c46-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-197">Click **Save**.</span></span>

14. <span data-ttu-id="30c46-198">Kattintson a **felhasználói** újra fülre.</span><span class="sxs-lookup"><span data-stu-id="30c46-198">Click the **User** tab again.</span></span>
    
    <span data-ttu-id="30c46-199">![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791926.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="30c46-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="30c46-200">Az a **attribútum** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="30c46-200">In the **Attribute** section, perform the following steps:</span></span>
    
    <span data-ttu-id="30c46-201">![Attribútum](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribútum")</span><span class="sxs-lookup"><span data-stu-id="30c46-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="30c46-202">a.</span><span class="sxs-lookup"><span data-stu-id="30c46-202">a.</span></span> <span data-ttu-id="30c46-203">Az a **objektumazonosító** szövegmezőhöz típus **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="30c46-203">In the **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="30c46-204">b.</span><span class="sxs-lookup"><span data-stu-id="30c46-204">b.</span></span> <span data-ttu-id="30c46-205">Az a **felhasználónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="30c46-205">In the **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="30c46-206">c.</span><span class="sxs-lookup"><span data-stu-id="30c46-206">c.</span></span> <span data-ttu-id="30c46-207">Az a **megjelenített név** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="30c46-207">In the **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="30c46-208">d.</span><span class="sxs-lookup"><span data-stu-id="30c46-208">d.</span></span> <span data-ttu-id="30c46-209">Az a **Utónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="30c46-209">In the **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="30c46-210">e.</span><span class="sxs-lookup"><span data-stu-id="30c46-210">e.</span></span> <span data-ttu-id="30c46-211">Az a **Vezetéknév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="30c46-211">In the **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="30c46-212">f.</span><span class="sxs-lookup"><span data-stu-id="30c46-212">f.</span></span> <span data-ttu-id="30c46-213">Az a **E-mail** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="30c46-213">In the **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="30c46-214">g.</span><span class="sxs-lookup"><span data-stu-id="30c46-214">g.</span></span> <span data-ttu-id="30c46-215">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="30c46-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="30c46-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="30c46-217">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="30c46-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="30c46-219">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="30c46-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="30c46-220">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="30c46-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="30c46-222">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="30c46-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="30c46-224">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="30c46-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="30c46-226">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="30c46-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="30c46-228">a.</span><span class="sxs-lookup"><span data-stu-id="30c46-228">a.</span></span> <span data-ttu-id="30c46-229">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30c46-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="30c46-230">b.</span><span class="sxs-lookup"><span data-stu-id="30c46-230">b.</span></span> <span data-ttu-id="30c46-231">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30c46-231">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="30c46-232">c.</span><span class="sxs-lookup"><span data-stu-id="30c46-232">c.</span></span> <span data-ttu-id="30c46-233">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="30c46-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="30c46-234">d.</span><span class="sxs-lookup"><span data-stu-id="30c46-234">d.</span></span> <span data-ttu-id="30c46-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="30c46-236">AirWatch tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="30c46-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="30c46-237">Ahhoz, hogy az Azure AD-felhasználók AirWatch bejelentkezni, akkor ki kell építenie a AirWatch.</span><span class="sxs-lookup"><span data-stu-id="30c46-237">To enable Azure AD users to log in to AirWatch, they must be provisioned in to AirWatch.</span></span>

* <span data-ttu-id="30c46-238">AirWatch, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="30c46-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="30c46-239">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="30c46-239">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="30c46-240">Jelentkezzen be a **AirWatch** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="30c46-240">Log in to your **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="30c46-241">A bal oldali navigációs ablaktábláján kattintson **fiókok**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="30c46-241">In the navigation pane on the left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="30c46-242">![Felhasználók](./media/active-directory-saas-airwatch-tutorial/ic791929.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="30c46-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="30c46-243">Az a **felhasználók** menüben kattintson a **listanézet**, és kattintson a **Hozzáadás \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="30c46-243">In the **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="30c46-244">![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791930.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="30c46-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="30c46-245">Az a **hozzáadása / szerkesztése felhasználói** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="30c46-245">On the **Add / Edit User** dialog, perform the following steps:</span></span>

   <span data-ttu-id="30c46-246">![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791931.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="30c46-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="30c46-247">Típus a **felhasználónév**, **jelszó**, **jelszó megerősítése**, **Utónév**, **keresztneve**,  **E-mail cím** egy érvényes Azure Active Directory-fiók szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="30c46-247">Type the **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="30c46-248">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="30c46-249">Bármely más AirWatch felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz AirWatch által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="30c46-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="30c46-250">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="30c46-250">Assigning the Azure AD test user</span></span>

<span data-ttu-id="30c46-251">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés AirWatch Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="30c46-251">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AirWatch.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="30c46-253">**Britta Simon hozzárendelése AirWatch, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="30c46-253">**To assign Britta Simon to AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="30c46-254">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30c46-254">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="30c46-256">Az alkalmazások listában válassza ki a **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="30c46-256">In the applications list, select **AirWatch**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="30c46-258">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="30c46-258">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="30c46-260">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="30c46-260">Click **Add** button.</span></span> <span data-ttu-id="30c46-261">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30c46-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="30c46-263">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="30c46-263">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="30c46-264">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30c46-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="30c46-265">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30c46-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="30c46-266">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="30c46-266">Testing single sign-on</span></span>

<span data-ttu-id="30c46-267">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="30c46-267">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="30c46-268">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="30c46-268">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="30c46-269">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="30c46-269">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="30c46-270">További források</span><span class="sxs-lookup"><span data-stu-id="30c46-270">Additional resources</span></span>

* [<span data-ttu-id="30c46-271">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="30c46-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30c46-272">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="30c46-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

