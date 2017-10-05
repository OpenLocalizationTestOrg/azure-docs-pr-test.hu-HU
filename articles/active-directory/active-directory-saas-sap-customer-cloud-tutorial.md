---
title: "Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél |} Microsoft Docs"
description: "Útmutató: az egyszeri bejelentkezés Azure Active Directory és az SAP-felhők közötti ügyfél konfigurálása."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: e4d945525a45704f34e1d9e742220928a516f341
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="901f4-103">Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél</span><span class="sxs-lookup"><span data-stu-id="901f4-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="901f4-104">Ebben az oktatóanyagban elsajátíthatja SAP felhő integrálása az Azure Active Directoryval (Azure AD-) ügyfél.</span><span class="sxs-lookup"><span data-stu-id="901f4-104">In this tutorial, you learn how to integrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="901f4-105">SAP felhő ügyfél és az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="901f4-105">Integrating SAP Cloud for Customer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="901f4-106">Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik az SAP felhőbe ügyfél</span><span class="sxs-lookup"><span data-stu-id="901f4-106">You can control in Azure AD who has access to SAP Cloud for Customer</span></span>
- <span data-ttu-id="901f4-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SAP felhőbe (egyszeri bejelentkezés) ügyfél a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="901f4-107">You can enable your users to automatically get signed-on to SAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="901f4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="901f4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="901f4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="901f4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="901f4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="901f4-110">Prerequisites</span></span>

<span data-ttu-id="901f4-111">Konfigurálhatja az Azure AD-integrációs SAP felhő a felhasználói, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="901f4-111">To configure Azure AD integration with SAP Cloud for Customer, you need the following items:</span></span>

- <span data-ttu-id="901f4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="901f4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="901f4-113">Az ügyfél egyszeri bejelentkezés SAP felhő előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="901f4-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="901f4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="901f4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="901f4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="901f4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="901f4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="901f4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="901f4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="901f4-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="901f4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="901f4-118">Scenario description</span></span>
<span data-ttu-id="901f4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="901f4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="901f4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="901f4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="901f4-121">SAP felhő hozzáadása az ügyfél a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="901f4-121">Adding SAP Cloud for Customer from the gallery</span></span>
2. <span data-ttu-id="901f4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="901f4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a><span data-ttu-id="901f4-123">SAP felhő hozzáadása az ügyfél a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="901f4-123">Adding SAP Cloud for Customer from the gallery</span></span>
<span data-ttu-id="901f4-124">SAP felhő integrálása az Azure AD-be ügyfél konfigurálásához kell hozzáadnia SAP felhő ügyfél a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="901f4-124">To configure the integration of SAP Cloud for Customer into Azure AD, you need to add SAP Cloud for Customer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="901f4-125">**Adja hozzá a SAP felhő ügyfél a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="901f4-125">**To add SAP Cloud for Customer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="901f4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="901f4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="901f4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="901f4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="901f4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="901f4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="901f4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="901f4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="901f4-133">Írja be a keresőmezőbe, **SAP felhő ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="901f4-133">In the search box, type **SAP Cloud for Customer**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="901f4-135">Az eredmények panelen válassza ki a **SAP felhő ügyfél**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="901f4-135">In the results panel, select **SAP Cloud for Customer**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="901f4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="901f4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="901f4-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez a SAP felhőalapú ügyfél "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="901f4-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="901f4-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó SAP felhőben ügyfél Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="901f4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP Cloud for Customer is to a user in Azure AD.</span></span> <span data-ttu-id="901f4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó SAP felhőben ügyfél közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="901f4-140">In other words, a link relationship between an Azure AD user and the related user in SAP Cloud for Customer needs to be established.</span></span>

<span data-ttu-id="901f4-141">Ügyfél felhőben SAP, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="901f4-141">In SAP Cloud for Customer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="901f4-142">Az Azure AD egyszeri bejelentkezéshez a SAP felhőalapú ügyfél tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="901f4-142">To configure and test Azure AD single sign-on with SAP Cloud for Customer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="901f4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="901f4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="901f4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="901f4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="901f4-145">**[Az ügyfél tesztfelhasználó SAP felhők létrehozásával](#creating-a-sap-cloud-for-customer-test-user)**  - ügyfél, amely csatolva van a felhasználó az Azure AD-ábrázolását rendelkezik egy megfelelője a Britta Simon SAP felhőben.</span><span class="sxs-lookup"><span data-stu-id="901f4-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - to have a counterpart of Britta Simon in SAP Cloud for Customer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="901f4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="901f4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="901f4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="901f4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="901f4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="901f4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="901f4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az SAP-felhőben felhasználói alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="901f4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="901f4-150">**Konfigurálja az Azure AD egyszeri bejelentkezést a SAP felhőalapú ügyfél, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="901f4-150">**To configure Azure AD single sign-on with SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="901f4-151">Az Azure portálon a a **SAP felhő ügyfél** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="901f4-151">In the Azure portal, on the **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="901f4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="901f4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="901f4-155">Az a **SAP felhő a felhasználói tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="901f4-155">On the **SAP Cloud for Customer Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="901f4-157">a.</span><span class="sxs-lookup"><span data-stu-id="901f4-157">a.</span></span> <span data-ttu-id="901f4-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="901f4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="901f4-159">b.</span><span class="sxs-lookup"><span data-stu-id="901f4-159">b.</span></span> <span data-ttu-id="901f4-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="901f4-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="901f4-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="901f4-161">These values are not real.</span></span> <span data-ttu-id="901f4-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="901f4-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="901f4-163">Ügyfél [SAP felhő ügyfél ügyfél-támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="901f4-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to get these values.</span></span> 

4. <span data-ttu-id="901f4-164">Az a **felhasználói attribútumok** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="901f4-164">On the **User Attributes** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="901f4-166">a.</span><span class="sxs-lookup"><span data-stu-id="901f4-166">a.</span></span> <span data-ttu-id="901f4-167">A **felhasználói azonosító** listáról válassza ki a **ExtractMailPrefix()** függvény.</span><span class="sxs-lookup"><span data-stu-id="901f4-167">In **User Identifier** list, select the **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="901f4-168">b.</span><span class="sxs-lookup"><span data-stu-id="901f4-168">b.</span></span> <span data-ttu-id="901f4-169">Az a **Mail** listára, válassza ki a megvalósítás a használni kívánt felhasználói attribútum.</span><span class="sxs-lookup"><span data-stu-id="901f4-169">From the **Mail** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="901f4-170">Például ha az egyedi felhasználói azonosítóval az EmployeeID használni kívánt, és az attribútum értékét a ExtensionAttribute2 tárolt, majd válassza ki user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="901f4-170">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="901f4-171">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="901f4-171">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="901f4-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="901f4-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="901f4-175">A a **SAP felhő a felhasználói konfigurálása** kattintson **SAP felhő konfigurálása a felhasználói** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="901f4-175">On the **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="901f4-176">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="901f4-176">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="901f4-178">Ahhoz, hogy egyszeri Bejelentkezéses konfigurálva, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="901f4-178">To get SSO configured, perform the following steps:</span></span>
   
    <span data-ttu-id="901f4-179">a.</span><span class="sxs-lookup"><span data-stu-id="901f4-179">a.</span></span> <span data-ttu-id="901f4-180">Bejelentkezés SAP felhő a felhasználói portál rendszergazda jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="901f4-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="901f4-181">b.</span><span class="sxs-lookup"><span data-stu-id="901f4-181">b.</span></span> <span data-ttu-id="901f4-182">Keresse meg a **alkalmazás- és felhasználói gyakori feladatot** , és kattintson a **identitásszolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="901f4-182">Navigate to the **Application and User Management Common Task** and click the **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="901f4-183">c.</span><span class="sxs-lookup"><span data-stu-id="901f4-183">c.</span></span> <span data-ttu-id="901f4-184">Kattintson a **új identitásszolgáltató** válassza ki az Azure-portálról letöltött metaadatok XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="901f4-184">Click **New Identity Provider** and select the metadata XML file you have downloaded from the Azure portal.</span></span> <span data-ttu-id="901f4-185">Importálja a metaadatokat, a rendszer automatikusan feltölti a szükséges aláírási tanúsítvány és titkosítási tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="901f4-185">By importing the metadata, the system automatically uploads the required signature certificate and encryption certificate.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="901f4-187">d.</span><span class="sxs-lookup"><span data-stu-id="901f4-187">d.</span></span> <span data-ttu-id="901f4-188">Az Azure Active Directory szükséges az elem helyességi feltétel ügyfél szolgáltatás URL-címe a SAML-kérelmet, ezért az a **helyességi feltétel fogyasztói szolgáltatás URL-címek** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="901f4-188">Azure Active Directory requires the element Assertion Consumer Service URL in the SAML request, so select the **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="901f4-189">e.</span><span class="sxs-lookup"><span data-stu-id="901f4-189">e.</span></span> <span data-ttu-id="901f4-190">Kattintson a **egyszeri bejelentkezés aktiválása**.</span><span class="sxs-lookup"><span data-stu-id="901f4-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="901f4-191">f.</span><span class="sxs-lookup"><span data-stu-id="901f4-191">f.</span></span> <span data-ttu-id="901f4-192">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="901f4-192">Save your changes.</span></span>
   
    <span data-ttu-id="901f4-193">g.</span><span class="sxs-lookup"><span data-stu-id="901f4-193">g.</span></span> <span data-ttu-id="901f4-194">Kattintson a **a rendszer** fülre.</span><span class="sxs-lookup"><span data-stu-id="901f4-194">Click the **My System** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="901f4-196">h.</span><span class="sxs-lookup"><span data-stu-id="901f4-196">h.</span></span> <span data-ttu-id="901f4-197">A **Azure AD bejelentkezési URL-címen** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="901f4-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="901f4-199">i.</span><span class="sxs-lookup"><span data-stu-id="901f4-199">i.</span></span> <span data-ttu-id="901f4-200">Adja meg, hogy az alkalmazott manuálisan választhat naplózás a felhasználói Azonosítót és jelszót vagy az SSO kiválasztásával a **manuális Identity Provider kijelölés**.</span><span class="sxs-lookup"><span data-stu-id="901f4-200">Specify whether the employee can manually choose between logging on with user ID and password or SSO by selecting the **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="901f4-201">j.</span><span class="sxs-lookup"><span data-stu-id="901f4-201">j.</span></span> <span data-ttu-id="901f4-202">Az a **egyszeri bejelentkezési URL-cím** szakaszban adja meg, jelentkezzen be a rendszer az alkalmazottak által használandó URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="901f4-202">In the **SSO URL** section, specify the URL that should be used by your employees to sign on to the system.</span></span> 
    <span data-ttu-id="901f4-203">Az a **URL-cím küldött alkalmazott** listájában, a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="901f4-203">In the **URL Sent to Employee** list, you can choose between the following options:</span></span>
   
    <span data-ttu-id="901f4-204">**Nem egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="901f4-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="901f4-205">A rendszer küld a dolgozó csak a normál rendszer URL-cím.</span><span class="sxs-lookup"><span data-stu-id="901f4-205">The system sends only the normal system URL to the employee.</span></span> <span data-ttu-id="901f4-206">Az alkalmazott nem jelentkezik be egyszeri Bejelentkezést, és kell jelszó használata vagy a tanúsítvány helyette.</span><span class="sxs-lookup"><span data-stu-id="901f4-206">The employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="901f4-207">**EGYSZERI BEJELENTKEZÉSI URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="901f4-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="901f4-208">A rendszer küld a dolgozó csak az egyszeri bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="901f4-208">The system sends only the SSO URL to the employee.</span></span> <span data-ttu-id="901f4-209">Az alkalmazottak az SSO használatával jelentkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="901f4-209">The employee can log on using SSO.</span></span> <span data-ttu-id="901f4-210">Hitelesítési kérelmet a rendszer átirányítja az IdP keresztül.</span><span class="sxs-lookup"><span data-stu-id="901f4-210">Authentication request is redirected through the IdP.</span></span>
   
    <span data-ttu-id="901f4-211">**Automatikus kiválasztásához.**</span><span class="sxs-lookup"><span data-stu-id="901f4-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="901f4-212">Ha egyszeri Bejelentkezéssel nem aktív, a rendszer küld a normál rendszer URL-címet az alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="901f4-212">If SSO is not active, the system sends the normal system URL to the employee.</span></span> <span data-ttu-id="901f4-213">Ha egyszeri bejelentkezés aktív, a rendszer ellenőrzi, hogy az alkalmazottnak a jelszót.</span><span class="sxs-lookup"><span data-stu-id="901f4-213">If SSO is active, the system checks whether the employee has a password.</span></span> <span data-ttu-id="901f4-214">A jelszó nem érhető el, ha mind SSO és URL-címe nem-SSO kerülnek az alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="901f4-214">If a password is available, both SSO URL and Non-SSO URL are sent to the employee.</span></span> <span data-ttu-id="901f4-215">Azonban ha az alkalmazott nincs jelszava, csak az egyszeri bejelentkezési URL-címet kap az alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="901f4-215">However, if the employee has no password, only the SSO URL is sent to the employee.</span></span>
   
    <span data-ttu-id="901f4-216">k.</span><span class="sxs-lookup"><span data-stu-id="901f4-216">k.</span></span> <span data-ttu-id="901f4-217">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="901f4-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="901f4-218">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="901f4-218">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="901f4-219">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="901f4-219">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="901f4-220">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="901f4-220">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="901f4-221">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="901f4-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="901f4-222">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="901f4-222">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="901f4-224">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="901f4-224">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="901f4-225">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="901f4-225">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="901f4-227">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="901f4-227">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="901f4-229">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="901f4-229">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="901f4-231">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="901f4-231">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="901f4-233">a.</span><span class="sxs-lookup"><span data-stu-id="901f4-233">a.</span></span> <span data-ttu-id="901f4-234">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="901f4-234">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="901f4-235">b.</span><span class="sxs-lookup"><span data-stu-id="901f4-235">b.</span></span> <span data-ttu-id="901f4-236">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="901f4-236">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="901f4-237">c.</span><span class="sxs-lookup"><span data-stu-id="901f4-237">c.</span></span> <span data-ttu-id="901f4-238">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="901f4-238">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="901f4-239">d.</span><span class="sxs-lookup"><span data-stu-id="901f4-239">d.</span></span> <span data-ttu-id="901f4-240">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="901f4-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="901f4-241">SAP felhő a felhasználói tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="901f4-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="901f4-242">Ebben a szakaszban egy SAP felhő Britta Simon nevű ügyfél felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="901f4-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="901f4-243">Adjon együttműködve [SAP felhő a felhasználói támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) a felhasználók hozzáadása az SAP felhőben ügyfél platform.</span><span class="sxs-lookup"><span data-stu-id="901f4-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to add the users in the SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="901f4-244">Győződjön meg arról, hogy NameID értéke az a felhasználónév mező a SAP felhőben ügyfél platform egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="901f4-244">Please make sure that NameID value should match with the username field in the SAP Cloud for Customer platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="901f4-245">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="901f4-245">Assigning the Azure AD test user</span></span>

<span data-ttu-id="901f4-246">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SAP felhő ügyfél Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="901f4-246">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP Cloud for Customer.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="901f4-248">**Britta Simon hozzárendelése SAP felhő ügyfél, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="901f4-248">**To assign Britta Simon to SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="901f4-249">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="901f4-249">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="901f4-251">Az alkalmazások listában válassza ki a **SAP felhő ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="901f4-251">In the applications list, select **SAP Cloud for Customer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="901f4-253">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="901f4-253">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="901f4-255">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="901f4-255">Click **Add** button.</span></span> <span data-ttu-id="901f4-256">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901f4-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="901f4-258">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="901f4-258">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="901f4-259">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901f4-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="901f4-260">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901f4-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="901f4-261">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="901f4-261">Testing single sign-on</span></span>

<span data-ttu-id="901f4-262">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="901f4-262">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="901f4-263">Ha a felhasználói csempe a hozzáférési Panel az SAP felhő gombra kattint, meg kell beolvasása automatikusan bejelentkezett az SAP-felhőhöz felhasználói alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="901f4-263">When you click the SAP Cloud for Customer tile in the Access Panel, you should get automatically signed-on to your SAP Cloud for Customer application.</span></span>
<span data-ttu-id="901f4-264">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="901f4-264">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="901f4-265">További források</span><span class="sxs-lookup"><span data-stu-id="901f4-265">Additional resources</span></span>

* [<span data-ttu-id="901f4-266">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="901f4-266">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="901f4-267">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="901f4-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

