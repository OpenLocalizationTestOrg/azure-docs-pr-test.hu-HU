---
title: "Oktatóanyag: Azure Active Directoryval integrált Clever |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Clever között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 84082ff567e37d7fff80be9e089c67cfab911861
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a><span data-ttu-id="cd42a-103">Oktatóanyag: Azure Active Directoryval integrált Clever</span><span class="sxs-lookup"><span data-stu-id="cd42a-103">Tutorial: Azure Active Directory integration with Clever</span></span>

<span data-ttu-id="cd42a-104">Ebben az oktatóanyagban elsajátíthatja Clever integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cd42a-104">In this tutorial, you learn how to integrate Clever with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd42a-105">Clever integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="cd42a-105">Integrating Clever with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cd42a-106">Az Azure AD, aki hozzáfér Clever szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="cd42a-106">You can control in Azure AD who has access to Clever.</span></span>
- <span data-ttu-id="cd42a-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Clever (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="cd42a-107">You can enable your users to automatically get signed-on to Clever (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="cd42a-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="cd42a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="cd42a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd42a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd42a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd42a-110">Prerequisites</span></span>

<span data-ttu-id="cd42a-111">Konfigurálása az Azure AD-integrációs Clever, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="cd42a-111">To configure Azure AD integration with Clever, you need the following items:</span></span>

- <span data-ttu-id="cd42a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cd42a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd42a-113">Egy intelligens egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cd42a-113">A Clever single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd42a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="cd42a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd42a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="cd42a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd42a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cd42a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd42a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd42a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd42a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cd42a-118">Scenario description</span></span>
<span data-ttu-id="cd42a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cd42a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd42a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cd42a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd42a-121">A gyűjteményből Clever hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd42a-121">Adding Clever from the gallery</span></span>
2. <span data-ttu-id="cd42a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cd42a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clever-from-the-gallery"></a><span data-ttu-id="cd42a-123">A gyűjteményből Clever hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd42a-123">Adding Clever from the gallery</span></span>
<span data-ttu-id="cd42a-124">Az Azure AD integrálása a Clever konfigurálásához kell hozzáadnia Clever a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="cd42a-124">To configure the integration of Clever into Azure AD, you need to add Clever from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cd42a-125">**A gyűjteményből Clever hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cd42a-125">**To add Clever from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cd42a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="cd42a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cd42a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="cd42a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="cd42a-133">Írja be a keresőmezőbe, **Clever**, jelölje be **Clever** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd42a-133">In the search box, type **Clever**, select **Clever** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában intelligens](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="cd42a-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd42a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="cd42a-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Clever.</span><span class="sxs-lookup"><span data-stu-id="cd42a-136">In this section, you configure and test Azure AD single sign-on with Clever based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cd42a-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Clever a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="cd42a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Clever is to a user in Azure AD.</span></span> <span data-ttu-id="cd42a-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Clever közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cd42a-138">In other words, a link relationship between an Azure AD user and the related user in Clever needs to be established.</span></span>

<span data-ttu-id="cd42a-139">Clever, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="cd42a-139">In Clever, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cd42a-140">Az Azure AD egyszeri bejelentkezést a Clever tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="cd42a-140">To configure and test Azure AD single sign-on with Clever, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cd42a-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="cd42a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cd42a-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="cd42a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd42a-143">**[Intelligens tesztfelhasználó létrehozása](#create-a-clever-test-user)**  - való Britta Simon valami Clever, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="cd42a-143">**[Create a Clever test user](#create-a-clever-test-user)** - to have a counterpart of Britta Simon in Clever that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd42a-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cd42a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd42a-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cd42a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="cd42a-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd42a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="cd42a-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez intelligens alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="cd42a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clever application.</span></span>

<span data-ttu-id="cd42a-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Clever, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cd42a-148">**To configure Azure AD single sign-on with Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="cd42a-149">Az Azure portálon a a **Clever** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-149">In the Azure portal, on the **Clever** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="cd42a-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cd42a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. <span data-ttu-id="cd42a-153">Az a **intelligens tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cd42a-153">On the **Clever Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat intelligens tartomány- és URL-címek](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    <span data-ttu-id="cd42a-155">a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-155">a.</span></span> <span data-ttu-id="cd42a-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://clever.com/in/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="cd42a-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://clever.com/in/<companyname>`</span></span>

    <span data-ttu-id="cd42a-157">b.</span><span class="sxs-lookup"><span data-stu-id="cd42a-157">b.</span></span> <span data-ttu-id="cd42a-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://clever.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="cd42a-158">In the **Identifier** textbox, type a URL using the following pattern: `https://clever.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd42a-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="cd42a-159">These values are not real.</span></span> <span data-ttu-id="cd42a-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cd42a-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cd42a-161">Ügyfél [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cd42a-161">Contact [Clever Client support team](https://clever.com/about/contact/) to get these values.</span></span>

4. <span data-ttu-id="cd42a-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cd42a-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-clever-tutorial/tutorial_clever_certificate.png)

5. <span data-ttu-id="cd42a-164">Az intelligens alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="cd42a-164">The Clever application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>

    <span data-ttu-id="cd42a-165">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-165">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png) 

6. <span data-ttu-id="cd42a-167">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd42a-167">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="cd42a-168">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="cd42a-168">Attribute Name</span></span>  | <span data-ttu-id="cd42a-169">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="cd42a-169">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="cd42a-170">clever.Student.credentials.District\_felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cd42a-170">clever.student.credentials.district\_username</span></span>  | <span data-ttu-id="cd42a-171">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="cd42a-171">user.userprincipalname</span></span> |
    | <span data-ttu-id="cd42a-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="cd42a-172">Firstname</span></span>  | <span data-ttu-id="cd42a-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="cd42a-173">user.givenname</span></span> |
    | <span data-ttu-id="cd42a-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="cd42a-174">Lastname</span></span>  | <span data-ttu-id="cd42a-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="cd42a-175">user.surname</span></span> |    

    <span data-ttu-id="cd42a-176">a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-176">a.</span></span> <span data-ttu-id="cd42a-177">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd42a-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="cd42a-180">b.</span><span class="sxs-lookup"><span data-stu-id="cd42a-180">b.</span></span> <span data-ttu-id="cd42a-181">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="cd42a-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="cd42a-182">c.</span><span class="sxs-lookup"><span data-stu-id="cd42a-182">c.</span></span> <span data-ttu-id="cd42a-183">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="cd42a-183">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="cd42a-184">d.</span><span class="sxs-lookup"><span data-stu-id="cd42a-184">d.</span></span> <span data-ttu-id="cd42a-185">Hagyja a **Namespace** szövegbeviteli mező üres.</span><span class="sxs-lookup"><span data-stu-id="cd42a-185">Leave the **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="cd42a-186">d.</span><span class="sxs-lookup"><span data-stu-id="cd42a-186">d.</span></span> <span data-ttu-id="cd42a-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-187">Click **Ok**.</span></span>     

5. <span data-ttu-id="cd42a-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cd42a-190">Létrehozásához a **metaadatok** URL-címe, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd42a-190">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="cd42a-191">a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-191">a.</span></span> <span data-ttu-id="cd42a-192">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-192">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_appregistrations.png)
   
    <span data-ttu-id="cd42a-194">b.</span><span class="sxs-lookup"><span data-stu-id="cd42a-194">b.</span></span> <span data-ttu-id="cd42a-195">Kattintson a **végpontok** megnyitásához **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="cd42a-195">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpointicon.png)

    <span data-ttu-id="cd42a-197">c.</span><span class="sxs-lookup"><span data-stu-id="cd42a-197">c.</span></span> <span data-ttu-id="cd42a-198">Kattintson a Másolás gombra másolása **ÖSSZEVONÁSI METAADAT-dokumentum** URL-címet, és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="cd42a-198">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpoint.png)
     
    <span data-ttu-id="cd42a-200">d.</span><span class="sxs-lookup"><span data-stu-id="cd42a-200">d.</span></span> <span data-ttu-id="cd42a-201">Most lépjen a tulajdonságlapján **Clever** , és másolja a **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="cd42a-201">Now go to the property page of **Clever** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-clever-tutorial/tutorial_clever_appid.png)

    <span data-ttu-id="cd42a-203">e.</span><span class="sxs-lookup"><span data-stu-id="cd42a-203">e.</span></span> <span data-ttu-id="cd42a-204">Készítése a **metaadatainak URL-CÍMÉT** a következő minta használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="cd42a-204">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>   

9. <span data-ttu-id="cd42a-205">Egy másik webes böngészőablakban jelentkezzen be a vállalat intelligens webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cd42a-205">In a different web browser window, log in to your Clever company site as an administrator.</span></span>

10. <span data-ttu-id="cd42a-206">Az eszköztáron kattintson **azonnali bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-206">In the toolbar, click **Instant Login**.</span></span>

    <span data-ttu-id="cd42a-207">![Azonnali bejelentkezési](./media/active-directory-saas-clever-tutorial/ic798984.png "azonnali bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="cd42a-207">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant Login")</span></span>

11. <span data-ttu-id="cd42a-208">Az a **azonnali bejelentkezési** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cd42a-208">On the **Instant Login** page, perform the following steps:</span></span>
      
      <span data-ttu-id="cd42a-209">![Azonnali bejelentkezési](./media/active-directory-saas-clever-tutorial/ic798985.png "azonnali bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="cd42a-209">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant Login")</span></span>
      
      <span data-ttu-id="cd42a-210">a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-210">a.</span></span> <span data-ttu-id="cd42a-211">Típus a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-211">Type the **Login URL**.</span></span>
      
      >[!NOTE]
      ><span data-ttu-id="cd42a-212">A **bejelentkezési URL-cím** egy egyéni érték.</span><span class="sxs-lookup"><span data-stu-id="cd42a-212">The **Login URL** is a custom value.</span></span> <span data-ttu-id="cd42a-213">Ügyfél [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="cd42a-213">Contact [Clever Client support team](https://clever.com/about/contact/) to get this value.</span></span>
      
      <span data-ttu-id="cd42a-214">b.</span><span class="sxs-lookup"><span data-stu-id="cd42a-214">b.</span></span> <span data-ttu-id="cd42a-215">Mint **Identitásrendszere**, jelölje be **az AD FS**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-215">As **Identity System**, select **ADFS**.</span></span>

      <span data-ttu-id="cd42a-216">c.</span><span class="sxs-lookup"><span data-stu-id="cd42a-216">c.</span></span> <span data-ttu-id="cd42a-217">Típus a **metaadatainak URL-CÍMÉT** a a **metaadatainak URL-CÍMÉT** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cd42a-217">Type the **Metadata URL** in the **Metadata URL** textbox.</span></span>
      
      <span data-ttu-id="cd42a-218">d.</span><span class="sxs-lookup"><span data-stu-id="cd42a-218">d.</span></span> <span data-ttu-id="cd42a-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-219">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="cd42a-220">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="cd42a-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cd42a-221">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="cd42a-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cd42a-222">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd42a-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="cd42a-223">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="cd42a-223">Create an Azure AD test user</span></span>

<span data-ttu-id="cd42a-224">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="cd42a-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="cd42a-226">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cd42a-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cd42a-227">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-227">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="cd42a-229">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-229">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="cd42a-231">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="cd42a-231">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="cd42a-233">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd42a-233">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    <span data-ttu-id="cd42a-235">a.</span><span class="sxs-lookup"><span data-stu-id="cd42a-235">a.</span></span> <span data-ttu-id="cd42a-236">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-236">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd42a-237">b.</span><span class="sxs-lookup"><span data-stu-id="cd42a-237">b.</span></span> <span data-ttu-id="cd42a-238">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd42a-238">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="cd42a-239">c.</span><span class="sxs-lookup"><span data-stu-id="cd42a-239">c.</span></span> <span data-ttu-id="cd42a-240">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="cd42a-240">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="cd42a-241">d.</span><span class="sxs-lookup"><span data-stu-id="cd42a-241">d.</span></span> <span data-ttu-id="cd42a-242">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-242">Click **Create**.</span></span>
 
### <a name="create-a-clever-test-user"></a><span data-ttu-id="cd42a-243">Intelligens tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd42a-243">Create a Clever test user</span></span>

<span data-ttu-id="cd42a-244">Ahhoz, hogy az Azure AD-felhasználók Clever bejelentkezni, akkor ki kell építenie a Clever.</span><span class="sxs-lookup"><span data-stu-id="cd42a-244">To enable Azure AD users to log in to Clever, they must be provisioned into Clever.</span></span>

<span data-ttu-id="cd42a-245">Használata esetén Clever, [intelligens ügyfél-támogatási csoport](https://clever.com/about/contact/) a felhasználók hozzáadása az intelligens platform.</span><span class="sxs-lookup"><span data-stu-id="cd42a-245">In case of Clever, Work with [Clever Client support team](https://clever.com/about/contact/) to add the users in the Clever platform.</span></span> <span data-ttu-id="cd42a-246">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="cd42a-246">Users must be created and activated before you use single sign-on.</span></span> 

>[!NOTE]
><span data-ttu-id="cd42a-247">Bármely más intelligens felhasználói fiók létrehozása eszközök vagy Clever kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="cd42a-247">You can use any other Clever user account creation tools or APIs provided by Clever to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="cd42a-248">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="cd42a-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="cd42a-249">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Clever Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="cd42a-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Clever.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="cd42a-251">**Britta Simon hozzárendelése Clever, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cd42a-251">**To assign Britta Simon to Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="cd42a-252">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cd42a-254">Az alkalmazások listában válassza ki a **Clever**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-254">In the applications list, select **Clever**.</span></span>

    ![Az alkalmazások listáját a Clever hivatkozásra](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)  

3. <span data-ttu-id="cd42a-256">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cd42a-256">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="cd42a-258">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd42a-258">Click **Add** button.</span></span> <span data-ttu-id="cd42a-259">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd42a-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="cd42a-261">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cd42a-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cd42a-262">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd42a-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd42a-263">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd42a-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="cd42a-264">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cd42a-264">Test single sign-on</span></span>

<span data-ttu-id="cd42a-265">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cd42a-265">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cd42a-266">Az intelligens csempe elemre a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett az intelligens alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="cd42a-266">When you click the Clever tile in the Access Panel, you should get automatically signed-on to your Clever application.</span></span>
<span data-ttu-id="cd42a-267">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cd42a-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cd42a-268">További források</span><span class="sxs-lookup"><span data-stu-id="cd42a-268">Additional resources</span></span>

* [<span data-ttu-id="cd42a-269">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="cd42a-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd42a-270">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cd42a-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

