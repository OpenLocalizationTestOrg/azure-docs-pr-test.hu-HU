---
title: "Oktatóanyag: Azure Active Directory-integráció Springer hivatkozás |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Springer hivatkozás között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: jeedes
ms.openlocfilehash: b9aec6f8f293cdd31456a7f50e3efe792804c7c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a><span data-ttu-id="c1604-103">Oktatóanyag: Azure Active Directory-integráció Springer hivatkozás</span><span class="sxs-lookup"><span data-stu-id="c1604-103">Tutorial: Azure Active Directory integration with Springer Link</span></span>

<span data-ttu-id="c1604-104">Ebben az oktatóanyagban elsajátíthatja Springer hivatkozás integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1604-104">In this tutorial, you learn how to integrate Springer Link with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1604-105">Springer hivatkozás integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c1604-105">Integrating Springer Link with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c1604-106">Azt is szabályozhatja az Azure AD, aki hozzáfér Springer hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c1604-106">You can control in Azure AD who has access to Springer Link.</span></span>
- <span data-ttu-id="c1604-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Springer hivatkozásra (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="c1604-107">You can enable your users to automatically get signed-on to Springer Link (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c1604-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="c1604-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="c1604-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1604-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1604-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1604-110">Prerequisites</span></span>

<span data-ttu-id="c1604-111">Az Azure AD-integráció konfigurálása a Springer hivatkozással, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c1604-111">To configure Azure AD integration with Springer Link, you need the following items:</span></span>

- <span data-ttu-id="c1604-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c1604-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1604-113">Egy Springer hivatkozás egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c1604-113">A Springer Link single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1604-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c1604-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1604-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c1604-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1604-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c1604-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1604-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1604-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1604-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c1604-118">Scenario description</span></span>
<span data-ttu-id="c1604-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c1604-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1604-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c1604-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1604-121">A gyűjteményből Springer hivatkozás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1604-121">Adding Springer Link from the gallery</span></span>
2. <span data-ttu-id="c1604-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c1604-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springer-link-from-the-gallery"></a><span data-ttu-id="c1604-123">A gyűjteményből Springer hivatkozás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1604-123">Adding Springer Link from the gallery</span></span>
<span data-ttu-id="c1604-124">Az Azure AD integrálása a Springer hivatkozás konfigurálásához kell hozzáadnia Springer hivatkozás a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c1604-124">To configure the integration of Springer Link into Azure AD, you need to add Springer Link from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c1604-125">**A gyűjteményből Springer hivatkozás hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1604-125">**To add Springer Link from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c1604-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1604-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="c1604-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c1604-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c1604-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1604-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="c1604-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c1604-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="c1604-133">Írja be a keresőmezőbe, **Springer hivatkozás**, jelölje be **Springer hivatkozás** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c1604-133">In the search box, type **Springer Link**, select **Springer Link** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában springer hivatkozás](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c1604-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1604-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c1604-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Springer hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c1604-136">In this section, you configure and test Azure AD single sign-on with Springer Link based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c1604-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Springer hivatkozás a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c1604-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Springer Link is to a user in Azure AD.</span></span> <span data-ttu-id="c1604-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Springer hivatkozás közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c1604-138">In other words, a link relationship between an Azure AD user and the related user in Springer Link needs to be established.</span></span>

<span data-ttu-id="c1604-139">Springer hivatkozásban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c1604-139">In Springer Link, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c1604-140">Az Azure AD az egyszeri bejelentkezés Springer hivatkozás, tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c1604-140">To configure and test Azure AD single sign-on with Springer Link, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c1604-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c1604-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c1604-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c1604-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1604-143">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c1604-143">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
4. <span data-ttu-id="c1604-144">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c1604-144">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c1604-145">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1604-145">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c1604-146">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Springer kapcsolat alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c1604-146">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Springer Link application.</span></span>

<span data-ttu-id="c1604-147">**Konfigurálja az Azure AD az egyszeri bejelentkezés Springer hivatkozás, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1604-147">**To configure Azure AD single sign-on with Springer Link, perform the following steps:**</span></span>

1. <span data-ttu-id="c1604-148">Az Azure portálon a a **Springer hivatkozás** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c1604-148">In the Azure portal, on the **Springer Link** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="c1604-150">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c1604-150">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_samlbase.png)

3. <span data-ttu-id="c1604-152">Az a **Springer hivatkozás tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="c1604-152">On the **Springer Link Domain and URLs** section,  If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk springer hivatkozás tartomány és az URL-címek](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url1.png)

    <span data-ttu-id="c1604-154">a.</span><span class="sxs-lookup"><span data-stu-id="c1604-154">a.</span></span> <span data-ttu-id="c1604-155">Az a **azonosító** szövegmező, írja be az URL-cím:`https://fsso.springer.com`</span><span class="sxs-lookup"><span data-stu-id="c1604-155">In the **Identifier** textbox, type the URL: `https://fsso.springer.com`</span></span>

    <span data-ttu-id="c1604-156">b.</span><span class="sxs-lookup"><span data-stu-id="c1604-156">b.</span></span> <span data-ttu-id="c1604-157">Az a **válasz URL-CÍMEN** szövegmező, írja be az URL-cím:`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="c1604-157">In the **Reply URL** textbox, type the URL: `https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

4. <span data-ttu-id="c1604-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="c1604-158">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="c1604-159">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="c1604-159">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk springer hivatkozás tartomány és az URL-címek](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url.png)

    <span data-ttu-id="c1604-161">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="c1604-161">In the **Sign-on URL** textbox, type the URL : `https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

5. <span data-ttu-id="c1604-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1604-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1604-164">Létrehozásához a **metaadatok** URL-címe, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c1604-164">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="c1604-165">a.</span><span class="sxs-lookup"><span data-stu-id="c1604-165">a.</span></span> <span data-ttu-id="c1604-166">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="c1604-166">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appregistrations.png)
   
    <span data-ttu-id="c1604-168">b.</span><span class="sxs-lookup"><span data-stu-id="c1604-168">b.</span></span> <span data-ttu-id="c1604-169">Kattintson a **végpontok** megnyitásához **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c1604-169">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpointicon.png)

    <span data-ttu-id="c1604-171">c.</span><span class="sxs-lookup"><span data-stu-id="c1604-171">c.</span></span> <span data-ttu-id="c1604-172">Kattintson a Másolás gombra másolása **ÖSSZEVONÁSI METAADAT-dokumentum** URL-címet, és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="c1604-172">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpoint.png)
     
    <span data-ttu-id="c1604-174">d.</span><span class="sxs-lookup"><span data-stu-id="c1604-174">d.</span></span> <span data-ttu-id="c1604-175">Most lépjen a tulajdonságlapján **Springer hivatkozás** , és másolja a **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="c1604-175">Now go to the property page of **Springer Link** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appid.png)

    <span data-ttu-id="c1604-177">e.</span><span class="sxs-lookup"><span data-stu-id="c1604-177">e.</span></span> <span data-ttu-id="c1604-178">Készítése a **metaadatainak URL-CÍMÉT** a következő minta használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="c1604-178">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="c1604-179">Egyszeri bejelentkezés konfigurálása **Springer hivatkozás** oldalon kell küldeniük a létrehozott **metaadatainak URL-CÍMÉT** való [Springer hivatkozás támogatási csoport](mailto:identity@springernature.com).</span><span class="sxs-lookup"><span data-stu-id="c1604-179">To configure single sign-on on **Springer Link** side, you need to send the generated **Metadata URL** to [Springer Link support team](mailto:identity@springernature.com).</span></span>

> [!TIP]
> <span data-ttu-id="c1604-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c1604-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c1604-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c1604-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c1604-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1604-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c1604-183">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c1604-183">Create an Azure AD test user</span></span>

<span data-ttu-id="c1604-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c1604-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="c1604-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1604-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c1604-187">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1604-187">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-springerlink-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c1604-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c1604-189">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-springerlink-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c1604-191">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c1604-191">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-springerlink-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c1604-193">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c1604-193">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-springerlink-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c1604-195">a.</span><span class="sxs-lookup"><span data-stu-id="c1604-195">a.</span></span> <span data-ttu-id="c1604-196">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1604-196">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1604-197">b.</span><span class="sxs-lookup"><span data-stu-id="c1604-197">b.</span></span> <span data-ttu-id="c1604-198">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1604-198">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c1604-199">c.</span><span class="sxs-lookup"><span data-stu-id="c1604-199">c.</span></span> <span data-ttu-id="c1604-200">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c1604-200">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c1604-201">d.</span><span class="sxs-lookup"><span data-stu-id="c1604-201">d.</span></span> <span data-ttu-id="c1604-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c1604-202">Click **Create**.</span></span>
 
### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c1604-203">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c1604-203">Assign the Azure AD test user</span></span>

<span data-ttu-id="c1604-204">Ebben a szakaszban engedélyezze Britta Simon használandó Azure egyszeri bejelentkezés által biztosított hozzáférés Springer hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c1604-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Springer Link.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="c1604-206">**Britta Simon hozzárendelése Springer hivatkozás, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1604-206">**To assign Britta Simon to Springer Link, perform the following steps:**</span></span>

1. <span data-ttu-id="c1604-207">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1604-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c1604-209">Az alkalmazások listában válassza ki a **Springer hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="c1604-209">In the applications list, select **Springer Link**.</span></span>

    ![Az alkalmazások listáját a Springer Link hivatkozás](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_app.png)  

3. <span data-ttu-id="c1604-211">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c1604-211">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="c1604-213">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1604-213">Click **Add** button.</span></span> <span data-ttu-id="c1604-214">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1604-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="c1604-216">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c1604-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c1604-217">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1604-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1604-218">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1604-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c1604-219">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c1604-219">Test single sign-on</span></span>

<span data-ttu-id="c1604-220">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c1604-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c1604-221">Ha a hozzáférési panelen Springer hivatkozás csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Springer hivatkozás alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c1604-221">When you click the Springer Link tile in the Access Panel, you should get automatically signed-on to your Springer Link application.</span></span>
<span data-ttu-id="c1604-222">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c1604-222">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1604-223">További források</span><span class="sxs-lookup"><span data-stu-id="c1604-223">Additional resources</span></span>

* [<span data-ttu-id="c1604-224">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c1604-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1604-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c1604-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_203.png

