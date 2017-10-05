---
title: "Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Qlik logika vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 4964634cd5aaf0dbb98c766f5e12700c4d118750
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="eadaa-103">Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati</span><span class="sxs-lookup"><span data-stu-id="eadaa-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="eadaa-104">Ebben az oktatóanyagban elsajátíthatja Qlik logika vállalati integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eadaa-104">In this tutorial, you learn how to integrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eadaa-105">Qlik logika vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="eadaa-105">Integrating Qlik Sense Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eadaa-106">Az Azure AD, aki hozzáfér Qlik logika vállalati szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="eadaa-106">You can control in Azure AD who has access to Qlik Sense Enterprise.</span></span>
- <span data-ttu-id="eadaa-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Qlik logika vállalati (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="eadaa-107">You can enable your users to automatically get signed-on to Qlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="eadaa-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="eadaa-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="eadaa-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eadaa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eadaa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eadaa-110">Prerequisites</span></span>

<span data-ttu-id="eadaa-111">Az Azure AD-integráció konfigurálása a Qlik logika vállalati, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="eadaa-111">To configure Azure AD integration with Qlik Sense Enterprise, you need the following items:</span></span>

- <span data-ttu-id="eadaa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="eadaa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eadaa-113">Egy Qlik logika vállalati egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="eadaa-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eadaa-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="eadaa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eadaa-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="eadaa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eadaa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="eadaa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eadaa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eadaa-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eadaa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="eadaa-118">Scenario description</span></span>
<span data-ttu-id="eadaa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="eadaa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eadaa-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="eadaa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eadaa-121">A gyűjteményből Qlik logika vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eadaa-121">Adding Qlik Sense Enterprise from the gallery</span></span>
2. <span data-ttu-id="eadaa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="eadaa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a><span data-ttu-id="eadaa-123">A gyűjteményből Qlik logika vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eadaa-123">Adding Qlik Sense Enterprise from the gallery</span></span>
<span data-ttu-id="eadaa-124">Az Azure AD integrálása a Qlik logika Enterprise konfigurálásához kell hozzáadnia Qlik logika vállalati a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="eadaa-124">To configure the integration of Qlik Sense Enterprise into Azure AD, you need to add Qlik Sense Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eadaa-125">**A gyűjteményből Qlik logika vállalati hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eadaa-125">**To add Qlik Sense Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eadaa-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="eadaa-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eadaa-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="eadaa-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="eadaa-133">Írja be a keresőmezőbe, **Qlik logika vállalati**, jelölje be **Qlik logika vállalati** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eadaa-133">In the search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Qlik logika Enterprise](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="eadaa-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eadaa-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="eadaa-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Qlik logika vállalati "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="eadaa-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eadaa-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Qlik logika vállalati a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="eadaa-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Qlik Sense Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="eadaa-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Qlik logika vállalati közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="eadaa-138">In other words, a link relationship between an Azure AD user and the related user in Qlik Sense Enterprise needs to be established.</span></span>

<span data-ttu-id="eadaa-139">Qlik logika vállalat, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="eadaa-139">In Qlik Sense Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="eadaa-140">Az Azure AD egyszeri bejelentkezést a Qlik logika vállalati tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="eadaa-140">To configure and test Azure AD single sign-on with Qlik Sense Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eadaa-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="eadaa-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eadaa-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="eadaa-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eadaa-143">**[Qlik logika vállalati tesztfelhasználó létrehozása](#create-a-qlik-sense-enterprise-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon Qlik logika vállalat, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="eadaa-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - to have a counterpart of Britta Simon in Qlik Sense Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="eadaa-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="eadaa-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eadaa-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="eadaa-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="eadaa-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eadaa-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="eadaa-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Qlik logika vállalati alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="eadaa-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="eadaa-148">**A Qlik logika vállalati konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eadaa-148">**To configure Azure AD single sign-on with Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="eadaa-149">Az Azure portálon a a **Qlik logika vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-149">In the Azure portal, on the **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="eadaa-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="eadaa-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="eadaa-153">Az a **Qlik logika vállalati tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eadaa-153">On the **Qlik Sense Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Qlik logika vállalati tartomány és az URL-címek](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="eadaa-155">a.</span><span class="sxs-lookup"><span data-stu-id="eadaa-155">a.</span></span> <span data-ttu-id="eadaa-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="eadaa-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="eadaa-157">Jegyezze fel ezt az URI vége a lezáró perjelet.</span><span class="sxs-lookup"><span data-stu-id="eadaa-157">Note the trailing slash at the end of this URI.</span></span> <span data-ttu-id="eadaa-158">Szükség.</span><span class="sxs-lookup"><span data-stu-id="eadaa-158">It is required.</span></span>
    
    <span data-ttu-id="eadaa-159">b.</span><span class="sxs-lookup"><span data-stu-id="eadaa-159">b.</span></span> <span data-ttu-id="eadaa-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="eadaa-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="eadaa-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="eadaa-161">These values are not real.</span></span> <span data-ttu-id="eadaa-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója, amelyeket ez az oktatóanyag, vagy forduljon a [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="eadaa-162">Update these values with the actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to get these values.</span></span> 

4. <span data-ttu-id="eadaa-163">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="eadaa-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="eadaa-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eadaa-167">Készítse elő az összevonási metaadatok XML-fájl, így feltöltheti, amely Qlik logika kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-167">Prepare the Federation Metadata XML file so that you can upload that to Qlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="eadaa-168">A kiállító terjesztési hely metaadatok feltöltését a Qlik logika kiszolgálóra, mielőtt az futtatásával távolítsa el az Azure AD között megfelelő működés érdekében lehet szerkeszteni kell a fájlt és Qlik logika kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="eadaa-168">Before uploading the IdP metadata to the Qlik Sense server, the file needs to be edited to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="eadaa-170">a.</span><span class="sxs-lookup"><span data-stu-id="eadaa-170">a.</span></span> <span data-ttu-id="eadaa-171">Nyissa meg az FederationMetaData.xml fájlt egy szövegszerkesztőben Azure portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="eadaa-171">Open the FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="eadaa-172">b.</span><span class="sxs-lookup"><span data-stu-id="eadaa-172">b.</span></span> <span data-ttu-id="eadaa-173">Keresse meg az érték **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-173">Search for the value **RoleDescriptor**.</span></span>  <span data-ttu-id="eadaa-174">Négy bejegyzést (nyitó és záró elemcímkék két pár) is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="eadaa-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="eadaa-175">c.</span><span class="sxs-lookup"><span data-stu-id="eadaa-175">c.</span></span> <span data-ttu-id="eadaa-176">Törölje a RoleDescriptor címkéket és az összes adatot a kettő között a fájlból.</span><span class="sxs-lookup"><span data-stu-id="eadaa-176">Delete the RoleDescriptor tags and all information in between from the file.</span></span>
   
    <span data-ttu-id="eadaa-177">d.</span><span class="sxs-lookup"><span data-stu-id="eadaa-177">d.</span></span> <span data-ttu-id="eadaa-178">Mentse a fájlt, és a dokumentum későbbi használatra közelben tartani.</span><span class="sxs-lookup"><span data-stu-id="eadaa-178">Save the file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="eadaa-179">Keresse meg a a Qlik logika Qlik felügyeleti konzol (QMC) hozhat létre virtuális proxybeállításokkal felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="eadaa-179">Navigate to the Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="eadaa-180">A QMC, kattintson a a **virtuális proxyk** menüpont.</span><span class="sxs-lookup"><span data-stu-id="eadaa-180">In the QMC, click on the **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="eadaa-182">A képernyő alján kattintson a **hozzon létre új** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-182">At the bottom of the screen, click the **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="eadaa-184">A virtuális proxy Szerkesztés képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-184">The Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="eadaa-185">A képernyő jobb oldalán van egy menüben adja meg a megfelelő konfigurációs beállítások láthatók.</span><span class="sxs-lookup"><span data-stu-id="eadaa-185">On the right side of the screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="eadaa-187">Az azonosító menü beállítás be van jelölve írja be az Azure virtuális proxykonfigurációt a azonosító adatokat.</span><span class="sxs-lookup"><span data-stu-id="eadaa-187">With the Identification menu option checked, enter the identifying information for the Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="eadaa-189">a.</span><span class="sxs-lookup"><span data-stu-id="eadaa-189">a.</span></span> <span data-ttu-id="eadaa-190">A **leírás** mező értéke egy rövid nevet a virtuális proxybeállításait.</span><span class="sxs-lookup"><span data-stu-id="eadaa-190">The **Description** field is a friendly name for the virtual proxy configuration.</span></span>  <span data-ttu-id="eadaa-191">Adjon meg egy értéket leírását.</span><span class="sxs-lookup"><span data-stu-id="eadaa-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="eadaa-192">b.</span><span class="sxs-lookup"><span data-stu-id="eadaa-192">b.</span></span> <span data-ttu-id="eadaa-193">A **előtag** mező azonosítja a virtuális proxy végpont Qlik logika az Azure AD egyszeri bejelentkezéshez történő kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="eadaa-193">The **Prefix** field identifies the virtual proxy endpoint for connecting to Qlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="eadaa-194">Adja meg a virtuális proxy előtag egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="eadaa-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="eadaa-195">c.</span><span class="sxs-lookup"><span data-stu-id="eadaa-195">c.</span></span> <span data-ttu-id="eadaa-196">**Munkamenet inaktivitás időkorlátja (perc)** az e virtuális proxyn keresztül történő kapcsolatok időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="eadaa-196">**Session inactivity timeout (minutes)** is the timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="eadaa-197">d.</span><span class="sxs-lookup"><span data-stu-id="eadaa-197">d.</span></span> <span data-ttu-id="eadaa-198">A **munkamenet cookie-k fejlécnév** tárolja a munkamenet-azonosító a sikeres hitelesítést követően a felhasználó kap Qlik logika munkamenet cookie-név.</span><span class="sxs-lookup"><span data-stu-id="eadaa-198">The **Session cookie header name** is the cookie name storing the session identifier for the Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="eadaa-199">Ez a név nem egyedi.</span><span class="sxs-lookup"><span data-stu-id="eadaa-199">This name must be unique.</span></span>

12. <span data-ttu-id="eadaa-200">Kattintson a hitelesítési menüpont láthatóvá.</span><span class="sxs-lookup"><span data-stu-id="eadaa-200">Click on the Authentication menu option to make it visible.</span></span>  <span data-ttu-id="eadaa-201">A hitelesítési képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-201">The Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="eadaa-203">a.</span><span class="sxs-lookup"><span data-stu-id="eadaa-203">a.</span></span> <span data-ttu-id="eadaa-204">A **névtelen hozzáférési mód** legördülő lista határozza meg, ha a névtelen felhasználók férhetnek hozzá Qlik logika virtuális proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="eadaa-204">The **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through the virtual proxy.</span></span>  <span data-ttu-id="eadaa-205">Az alapértelmezett beállítás nem névtelen felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eadaa-205">The default option is No anonymous user.</span></span>
    
    <span data-ttu-id="eadaa-206">b.</span><span class="sxs-lookup"><span data-stu-id="eadaa-206">b.</span></span> <span data-ttu-id="eadaa-207">A **hitelesítési módszer** legördülő lista határozza meg a hitelesítési séma a virtuális proxy fogja használni.</span><span class="sxs-lookup"><span data-stu-id="eadaa-207">The **Authentication method** drop-down determines the authentication scheme the virtual proxy will use.</span></span>  <span data-ttu-id="eadaa-208">A legördülő listából válassza ki a SAML-alapú.</span><span class="sxs-lookup"><span data-stu-id="eadaa-208">Select SAML from the drop-down list.</span></span>  <span data-ttu-id="eadaa-209">További beállítások következtében jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="eadaa-210">c.</span><span class="sxs-lookup"><span data-stu-id="eadaa-210">c.</span></span> <span data-ttu-id="eadaa-211">Az a **SAML gazdagép URI mező**, adjon meg az állomásnév felhasználók Qlik logika a SAML-alapú virtuális proxyn keresztül történő eléréséhez adja meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-211">In the **SAML host URI field**, input the hostname users enter to access Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="eadaa-212">Az állomásnév az uri a Qlik logika kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="eadaa-212">The hostname is the uri of the Qlik Sense server.</span></span>
    
    <span data-ttu-id="eadaa-213">d.</span><span class="sxs-lookup"><span data-stu-id="eadaa-213">d.</span></span> <span data-ttu-id="eadaa-214">Az a **SAML Entitásazonosító**, adja meg a SAML állomás URI mezőben megadott azonos érték.</span><span class="sxs-lookup"><span data-stu-id="eadaa-214">In the **SAML entity ID**, enter the same value entered for the SAML host URI field.</span></span>
    
    <span data-ttu-id="eadaa-215">e.</span><span class="sxs-lookup"><span data-stu-id="eadaa-215">e.</span></span> <span data-ttu-id="eadaa-216">A **SAML IdP metaadatok** a korábban a szerkesztett fájl a **összevonási metaadatok szerkesztése az Azure AD-konfigurációból** szakasz.</span><span class="sxs-lookup"><span data-stu-id="eadaa-216">The **SAML IdP metadata** is the file edited earlier in the **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="eadaa-217">**Mielőtt a kiállító terjesztési hely metaadatok feltöltése, a fájl szerkeszthető kell** futtatásával távolítsa el az Azure AD között megfelelő működés érdekében és Qlik logika kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="eadaa-217">**Before uploading the IdP metadata, the file needs to be edited** to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="eadaa-218">**Tekintse meg a fenti utasításokat kell-e a fájl még szerkeszthető.**</span><span class="sxs-lookup"><span data-stu-id="eadaa-218">**Please refer to the instructions above if the file has yet to be edited.**</span></span>  <span data-ttu-id="eadaa-219">Amennyiben a fájl kattintson a Tallózás gombra, és válassza ki a szerkesztett metaadatok fájlt feltölteni azt a virtuális proxykonfigurációt.</span><span class="sxs-lookup"><span data-stu-id="eadaa-219">If the file has been edited click on the Browse button and select the edited metadata file to upload it to the virtual proxy configuration.</span></span>
    
    <span data-ttu-id="eadaa-220">f.</span><span class="sxs-lookup"><span data-stu-id="eadaa-220">f.</span></span> <span data-ttu-id="eadaa-221">Adja meg a nevet vagy séma attribútumhivatkozás a SAML attribútum képviselő a **UserID** Qlik logika kiszolgálónak küldi az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eadaa-221">Enter the attribute name or schema reference for the SAML attribute representing the **UserID** Azure AD sends to the Qlik Sense server.</span></span>  <span data-ttu-id="eadaa-222">Az Azure alkalmazás képernyők feladás egy vagy több konfigurációs séma referencia jellegű információt érhető el.</span><span class="sxs-lookup"><span data-stu-id="eadaa-222">Schema reference information is available in the Azure app screens post configuration.</span></span>  <span data-ttu-id="eadaa-223">A name attribútum használatához adja meg a `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="eadaa-223">To use the name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="eadaa-224">g.</span><span class="sxs-lookup"><span data-stu-id="eadaa-224">g.</span></span> <span data-ttu-id="eadaa-225">Adja meg az értéket a **felhasználói directory** , amely csatolva lesz a felhasználók Qlik logika kiszolgálóhoz az Azure AD-n keresztül való hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="eadaa-225">Enter the value for the **user directory** that will be attached to users when they authenticate to Qlik Sense server through Azure AD.</span></span>  <span data-ttu-id="eadaa-226">Szoftveresen kötött értékeket kell lennie.%n **szögletes szögletes zárójelbe []**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="eadaa-227">Az Azure AD SAML-előfeltétel küldött attribútum használandó mezőben adja meg az attribútum neve a **nélkül** szögletes zárójelek között.</span><span class="sxs-lookup"><span data-stu-id="eadaa-227">To use an attribute sent in the Azure AD SAML assertion, enter the name of the attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="eadaa-228">h.</span><span class="sxs-lookup"><span data-stu-id="eadaa-228">h.</span></span> <span data-ttu-id="eadaa-229">A **SAML aláírási algoritmus** beállítja a szolgáltatás-szolgáltatója (a kis Qlik logika kiszolgáló) a tanúsítvány-aláírási virtuális proxy-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="eadaa-229">The **SAML signing algorithm** sets the service provider (in this case Qlik Sense server) certificate signing for the virtual proxy configuration.</span></span>  <span data-ttu-id="eadaa-230">Ha Qlik logika kiszolgáló Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató használatával létrehozott megbízható tanúsítványt használ, módosítsa a SAML aláírási algoritmus **SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change the SAML signing algorithm to **SHA-256**.</span></span>
    
    <span data-ttu-id="eadaa-231">i.</span><span class="sxs-lookup"><span data-stu-id="eadaa-231">i.</span></span> <span data-ttu-id="eadaa-232">A SAML attribútum leképezési szakasz lehetővé teszi további tulajdonságai, például a csoportok a biztonsági szabályok használható Qlik logika küldendő.</span><span class="sxs-lookup"><span data-stu-id="eadaa-232">The SAML attribute mapping section allows for additional attributes like groups to be sent to Qlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="eadaa-233">Kattintson a **TERHELÉSELOSZTÁS** láthatóvá menüjét.</span><span class="sxs-lookup"><span data-stu-id="eadaa-233">Click on the **LOAD BALANCING** menu option to make it visible.</span></span>  <span data-ttu-id="eadaa-234">A terheléselosztás képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-234">The Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="eadaa-236">Kattintson a a **új csomópont hozzáadása a kiszolgáló** gombra, jelölje be a vezérlő csomópont vagy csomópontok Qlik logika küldi a munkamenetek terheléselosztást terheléselosztási célra, és kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-236">Click on the **Add new server node** button, select engine node or nodes Qlik Sense will send sessions to for load balancing purposes, and click the **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="eadaa-238">Kattintson a speciális menüpont láthatóvá.</span><span class="sxs-lookup"><span data-stu-id="eadaa-238">Click on the Advanced menu option to make it visible.</span></span> <span data-ttu-id="eadaa-239">A Speciális képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-239">The Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="eadaa-241">A gazdagép fehér lista, amely a Qlik logika kiszolgálóhoz való csatlakozáskor az elfogadás állomásnevek azonosítja.</span><span class="sxs-lookup"><span data-stu-id="eadaa-241">The Host white list identifies hostnames that are accepted when connecting to the Qlik Sense server.</span></span>  <span data-ttu-id="eadaa-242">**Adja meg az állomásnevet kell megadnia a felhasználók Qlik logika kiszolgálóhoz való csatlakozáskor.**</span><span class="sxs-lookup"><span data-stu-id="eadaa-242">**Enter the hostname users will specify when connecting to Qlik Sense server.**</span></span> <span data-ttu-id="eadaa-243">Az állomásnév megadása értéke megegyezik a SAML állomás uri a https:// nélkül.</span><span class="sxs-lookup"><span data-stu-id="eadaa-243">The hostname is the same value as the SAML host uri without the https://.</span></span>

16. <span data-ttu-id="eadaa-244">Kattintson a **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-244">Click the **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="eadaa-246">Kattintson az OK gombra, fogadja el a figyelmeztető üzenet arról, hogy a virtuális proxy kapcsolódó proxyk újraindul.</span><span class="sxs-lookup"><span data-stu-id="eadaa-246">Click OK to accept the warning message that states proxies linked to the virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="eadaa-248">A képernyő jobb oldalán megjelenik a társított elem menü.</span><span class="sxs-lookup"><span data-stu-id="eadaa-248">On the right side of the screen, the Associated items menu appears.</span></span>  <span data-ttu-id="eadaa-249">Kattintson a **proxyk** menüjét.</span><span class="sxs-lookup"><span data-stu-id="eadaa-249">Click on the **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="eadaa-251">A proxy képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-251">The proxy screen appears.</span></span>  <span data-ttu-id="eadaa-252">Kattintson a **hivatkozás** panel alján a proxy csatolása a virtuális proxy.</span><span class="sxs-lookup"><span data-stu-id="eadaa-252">Click the **Link** button at the bottom to link a proxy to the virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="eadaa-254">Jelölje ki a proxy csomópontot, amely támogatja a virtuális proxy kapcsolat, és kattintson a **hivatkozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-254">Select the proxy node that will support this virtual proxy connection and click the **Link** button.</span></span>  <span data-ttu-id="eadaa-255">Linking, miután a proxy a társított proxyk alatt jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-255">After linking, the proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="eadaa-258">Körülbelül öt-tíz számítógépből álló másodperc után a frissítési QMC üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="eadaa-258">After about five to ten seconds, the Refresh QMC message will appear.</span></span>  <span data-ttu-id="eadaa-259">Kattintson a **frissítése QMC** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-259">Click the **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="eadaa-261">Amikor frissíti a QMC, kattintson a a **virtuális proxyk** menüpont.</span><span class="sxs-lookup"><span data-stu-id="eadaa-261">When the QMC refreshes, click on the **Virtual proxies** menu item.</span></span> <span data-ttu-id="eadaa-262">Az új SAML-alapú virtuális proxy bejegyzést a képernyőn a táblázatban szerepel.</span><span class="sxs-lookup"><span data-stu-id="eadaa-262">The new SAML virtual proxy entry is listed in the table on the screen.</span></span>  <span data-ttu-id="eadaa-263">A virtuális proxy bejegyzésre egyetlen kattintással.</span><span class="sxs-lookup"><span data-stu-id="eadaa-263">Single click on the virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="eadaa-265">A képernyő alján a letöltés SP metaadatok gomb aktiválódik.</span><span class="sxs-lookup"><span data-stu-id="eadaa-265">At the bottom of the screen, the Download SP metadata button will activate.</span></span>  <span data-ttu-id="eadaa-266">Kattintson a **letöltése SP metaadatok** gombra kattintva a metaadatok mentése fájlba.</span><span class="sxs-lookup"><span data-stu-id="eadaa-266">Click the **Download SP metadata** button to save the metadata to a file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="eadaa-268">Nyissa meg az sp metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="eadaa-268">Open the sp metadata file.</span></span>  <span data-ttu-id="eadaa-269">Figyelje meg a **entityid beállítást** bejegyzést, és a **AssertionConsumerService** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="eadaa-269">Observe the **entityID** entry and the **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="eadaa-270">Ezek az értékek egyenértékűek a **azonosító** és a **bejelentkezési URL-cím** az Azure AD alkalmazás konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="eadaa-270">These values are equivalent to the **Identifier** and the **Sign on URL** in the Azure AD application configuration.</span></span> <span data-ttu-id="eadaa-271">Ezek az értékek illessze be a **Qlik logika vállalati tartomány és az URL-címek** szakaszban az Azure AD alkalmazás konfigurációjának, ha azok nem egyeznek, akkor le kell cserélni őket az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="eadaa-271">Paste these values in the **Qlik Sense Enterprise Domain and URLs** section in the Azure AD application configuration if they are not matching, then you should replace them in the Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="eadaa-273">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="eadaa-273">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="eadaa-274">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="eadaa-274">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="eadaa-275">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eadaa-275">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="eadaa-276">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eadaa-276">Create an Azure AD test user</span></span>
<span data-ttu-id="eadaa-277">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="eadaa-277">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="eadaa-279">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eadaa-279">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eadaa-280">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-280">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

   ![Az Azure Active Directory gomb](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="eadaa-282">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-282">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

   ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="eadaa-284">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="eadaa-284">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

   ![A Hozzáadás gombra.](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="eadaa-286">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="eadaa-286">In the **User** dialog box, perform the following steps:</span></span>

   ![A felhasználó párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="eadaa-288">a.</span><span class="sxs-lookup"><span data-stu-id="eadaa-288">a.</span></span> <span data-ttu-id="eadaa-289">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-289">In the **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="eadaa-290">b.</span><span class="sxs-lookup"><span data-stu-id="eadaa-290">b.</span></span> <span data-ttu-id="eadaa-291">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eadaa-291">In the **User name** box, type the email address of user Britta Simon.</span></span>

   <span data-ttu-id="eadaa-292">c.</span><span class="sxs-lookup"><span data-stu-id="eadaa-292">c.</span></span> <span data-ttu-id="eadaa-293">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="eadaa-293">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

   <span data-ttu-id="eadaa-294">d.</span><span class="sxs-lookup"><span data-stu-id="eadaa-294">d.</span></span> <span data-ttu-id="eadaa-295">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="eadaa-296">Qlik logika vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eadaa-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="eadaa-297">Ebben a szakaszban Britta Simon meghívta Qlik logika vállalati felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="eadaa-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="eadaa-298">Együttműködve [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) a felhasználók hozzáadása a Qlik logika vállalati platform.</span><span class="sxs-lookup"><span data-stu-id="eadaa-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add the users in the Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="eadaa-299">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="eadaa-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="eadaa-300">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eadaa-300">Assign the Azure AD test user</span></span>

<span data-ttu-id="eadaa-301">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Qlik logika vállalati Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="eadaa-301">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Qlik Sense Enterprise.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="eadaa-303">**Britta Simon hozzárendelése Qlik logika vállalati, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eadaa-303">**To assign Britta Simon to Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="eadaa-304">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-304">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="eadaa-306">Az alkalmazások listában válassza ki a **Qlik logika vállalati**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-306">In the applications list, select **Qlik Sense Enterprise**.</span></span>

    ![Az alkalmazások listáját a Qlik logika vállalati hivatkozás](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="eadaa-308">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="eadaa-308">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="eadaa-310">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eadaa-310">Click **Add** button.</span></span> <span data-ttu-id="eadaa-311">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eadaa-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="eadaa-313">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="eadaa-313">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eadaa-314">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eadaa-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eadaa-315">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eadaa-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="eadaa-316">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="eadaa-316">Test single sign-on</span></span>

<span data-ttu-id="eadaa-317">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="eadaa-317">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="eadaa-318">Ha a hozzáférési panelen Qlik logika vállalati csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Qlik logika vállalati alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="eadaa-318">When you click the Qlik Sense Enterprise tile in the Access Panel, you should get automatically signed-on to your Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="eadaa-319">További források</span><span class="sxs-lookup"><span data-stu-id="eadaa-319">Additional resources</span></span>

* [<span data-ttu-id="eadaa-320">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="eadaa-320">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eadaa-321">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="eadaa-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

