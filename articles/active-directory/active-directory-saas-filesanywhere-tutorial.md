---
title: "Oktatóanyag: Azure Active Directoryval integrált FilesAnywhere |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és FilesAnywhere között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 4153056bd21006061c6ad8ff9cf3c17de9248628
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="b1895-103">Oktatóanyag: Azure Active Directoryval integrált FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="b1895-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="b1895-104">Ebben az oktatóanyagban elsajátíthatja FilesAnywhere integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1895-104">In this tutorial, you learn how to integrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1895-105">FilesAnywhere integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b1895-105">Integrating FilesAnywhere with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b1895-106">Megadhatja a FilesAnywhere hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b1895-106">You can control in Azure AD who has access to FilesAnywhere</span></span>
- <span data-ttu-id="b1895-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett FilesAnywhere (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b1895-107">You can enable your users to automatically get signed-on to FilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1895-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="b1895-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="b1895-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1895-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1895-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b1895-110">Prerequisites</span></span>

<span data-ttu-id="b1895-111">Konfigurálása az Azure AD-integrációs FilesAnywhere, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b1895-111">To configure Azure AD integration with FilesAnywhere, you need the following items:</span></span>

- <span data-ttu-id="b1895-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b1895-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1895-113">Egy FilesAnywhere egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b1895-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="b1895-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b1895-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="b1895-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b1895-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1895-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1895-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b1895-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1895-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="b1895-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b1895-118">Scenario description</span></span>
<span data-ttu-id="b1895-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b1895-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1895-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b1895-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1895-121">A gyűjteményből FilesAnywhere hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1895-121">Adding FilesAnywhere from the gallery</span></span>
2. <span data-ttu-id="b1895-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1895-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-the-gallery"></a><span data-ttu-id="b1895-123">A gyűjteményből FilesAnywhere hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1895-123">Adding FilesAnywhere from the gallery</span></span>
<span data-ttu-id="b1895-124">Az Azure AD integrálása a FilesAnywhere konfigurálásához kell hozzáadnia FilesAnywhere a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b1895-124">To configure the integration of FilesAnywhere into Azure AD, you need to add FilesAnywhere from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b1895-125">**A gyűjteményből FilesAnywhere hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b1895-125">**To add FilesAnywhere from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b1895-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1895-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1895-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b1895-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b1895-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1895-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b1895-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b1895-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b1895-133">Írja be a keresőmezőbe, **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="b1895-133">In the search box, type **FilesAnywhere**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="b1895-135">Az eredmények panelen válassza ki a **FilesAnywhere**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b1895-135">In the results panel, select **FilesAnywhere**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1895-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1895-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1895-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="b1895-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b1895-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó FilesAnywhere a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b1895-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FilesAnywhere is to a user in Azure AD.</span></span> <span data-ttu-id="b1895-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a FilesAnywhere közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b1895-140">In other words, a link relationship between an Azure AD user and the related user in FilesAnywhere needs to be established.</span></span>

<span data-ttu-id="b1895-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** FilesAnywhere a.</span><span class="sxs-lookup"><span data-stu-id="b1895-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="b1895-142">Az Azure AD egyszeri bejelentkezést a FilesAnywhere tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b1895-142">To configure and test Azure AD single sign-on with FilesAnywhere, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b1895-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b1895-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b1895-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b1895-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1895-145">**[FilesAnywhere tesztfelhasználó létrehozása](#creating-a-filesanywhere-test-user)**  - való Britta Simon valami FilesAnywhere, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b1895-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - to have a counterpart of Britta Simon in FilesAnywhere that is linked to the Azure AD representation of her.</span></span>
3. <span data-ttu-id="b1895-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b1895-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
4. <span data-ttu-id="b1895-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b1895-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1895-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b1895-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1895-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az FilesAnywhere alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b1895-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="b1895-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés FilesAnywhere, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b1895-150">**To configure Azure AD single sign-on with FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="b1895-151">Az Azure felügyeleti portálján a a **FilesAnywhere** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b1895-151">In the Azure Management portal, on the **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b1895-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="b1895-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="b1895-155">Az a **FilesAnywhere tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="b1895-155">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="b1895-157">a.</span><span class="sxs-lookup"><span data-stu-id="b1895-157">a.</span></span> <span data-ttu-id="b1895-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="b1895-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="b1895-159">Ne feledje, hogy az érték **215** van egy **clientid** és csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="b1895-159">Please note that the value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="b1895-160">Cserélje le a tényleges clientid értéket kell.</span><span class="sxs-lookup"><span data-stu-id="b1895-160">You need to replace it with the actual clientid value.</span></span>

4. <span data-ttu-id="b1895-161">Az a **FilesAnywhere tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b1895-161">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="b1895-163">a.</span><span class="sxs-lookup"><span data-stu-id="b1895-163">a.</span></span> <span data-ttu-id="b1895-164">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="b1895-164">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="b1895-165">b.</span><span class="sxs-lookup"><span data-stu-id="b1895-165">b.</span></span> <span data-ttu-id="b1895-166">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="b1895-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1895-167">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="b1895-167">Please note that these are not the real values.</span></span> <span data-ttu-id="b1895-168">Akkor frissítheti ezeket az értékeket a tényleges URL-cím bejelentkezési és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="b1895-168">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="b1895-169">Ügyfél [FilesAnywhere támogatási csoport](mailto:support@FilesAnywhere.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b1895-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) to get these values.</span></span> 

5. <span data-ttu-id="b1895-170">FilesAnywhere alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="b1895-170">FilesAnywhere Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="b1895-171">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b1895-171">Please configure the following claims for this application.</span></span> <span data-ttu-id="b1895-172">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="b1895-172">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="b1895-173">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="b1895-173">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="b1895-175">Amikor a felhasználók előfizet az FilesAnywhere értékének beolvasása **clientid** attribútumot [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="b1895-175">When the users signs up with FilesAnywhere they get the value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="b1895-176">Fel kell vennie az "Ügyfél-azonosító" attribútum FilesAnywhere által biztosított egyedi értékkel.</span><span class="sxs-lookup"><span data-stu-id="b1895-176">You have to add the "Client Id" attribute with the unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="b1895-177">A fent látható összes attribútum megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="b1895-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="b1895-178">Ne feledje, hogy az érték **2331** a **clientid** csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="b1895-178">Please note that the value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="b1895-179">Meg kell adnia a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="b1895-179">You need to provide the actual value.</span></span>


6. <span data-ttu-id="b1895-180">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b1895-180">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="b1895-181">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b1895-181">Attribute Name</span></span> | <span data-ttu-id="b1895-182">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b1895-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="b1895-183">ClientID</span><span class="sxs-lookup"><span data-stu-id="b1895-183">clientid</span></span> | <span data-ttu-id="b1895-184">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="b1895-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="b1895-185">a.</span><span class="sxs-lookup"><span data-stu-id="b1895-185">a.</span></span> <span data-ttu-id="b1895-186">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1895-186">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="b1895-189">b.</span><span class="sxs-lookup"><span data-stu-id="b1895-189">b.</span></span> <span data-ttu-id="b1895-190">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="b1895-190">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b1895-191">c.</span><span class="sxs-lookup"><span data-stu-id="b1895-191">c.</span></span> <span data-ttu-id="b1895-192">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="b1895-192">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b1895-193">d.</span><span class="sxs-lookup"><span data-stu-id="b1895-193">d.</span></span> <span data-ttu-id="b1895-194">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="b1895-194">Click **Ok**</span></span>

7. <span data-ttu-id="b1895-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1895-195">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b1895-197">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b1895-197">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="b1895-199">A a **FilesAnywhere konfigurációs** kattintson **konfigurálása FilesAnywhere** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b1895-199">On the **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** to open **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="b1895-202">Ahhoz, hogy egyszeri Bejelentkezéses konfigurációs teljes FilesAnywhere végén az alkalmazás, lépjen kapcsolatba [FilesAnywhere támogatási csoport](mailto:support@FilesAnywhere.com) és adja meg a letöltött SAML jogkivonat-aláíró tanúsítvány és az egyszeri bejelentkezés (SSO) URL-címet.</span><span class="sxs-lookup"><span data-stu-id="b1895-202">To get SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them the downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1895-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1895-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1895-204">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b1895-204">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b1895-206">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b1895-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b1895-207">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1895-207">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1895-209">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b1895-209">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1895-211">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1895-211">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1895-213">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b1895-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1895-215">a.</span><span class="sxs-lookup"><span data-stu-id="b1895-215">a.</span></span> <span data-ttu-id="b1895-216">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1895-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1895-217">b.</span><span class="sxs-lookup"><span data-stu-id="b1895-217">b.</span></span> <span data-ttu-id="b1895-218">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1895-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1895-219">c.</span><span class="sxs-lookup"><span data-stu-id="b1895-219">c.</span></span> <span data-ttu-id="b1895-220">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b1895-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b1895-221">d.</span><span class="sxs-lookup"><span data-stu-id="b1895-221">d.</span></span> <span data-ttu-id="b1895-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b1895-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="b1895-223">FilesAnywhere tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1895-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="b1895-224">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="b1895-224">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b1895-225">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b1895-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b1895-226">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés FilesAnywhere Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b1895-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to FilesAnywhere.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b1895-228">**Britta Simon hozzárendelése FilesAnywhere, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b1895-228">**To assign Britta Simon to FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="b1895-229">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1895-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b1895-231">Az alkalmazások listában válassza ki a **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="b1895-231">In the applications list, select **FilesAnywhere**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="b1895-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b1895-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b1895-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1895-235">Click **Add** button.</span></span> <span data-ttu-id="b1895-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1895-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b1895-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b1895-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b1895-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1895-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1895-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1895-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="b1895-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b1895-241">Testing single sign-on</span></span>

<span data-ttu-id="b1895-242">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b1895-242">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b1895-243">Ha a hozzáférési panelen FilesAnywhere csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az FilesAnywhere alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b1895-243">When you click the FilesAnywhere tile in the Access Panel, you should get automatically signed-on to your FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b1895-244">További források</span><span class="sxs-lookup"><span data-stu-id="b1895-244">Additional resources</span></span>

* [<span data-ttu-id="b1895-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b1895-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1895-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b1895-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
