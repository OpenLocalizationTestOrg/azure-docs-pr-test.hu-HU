---
title: "Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az Online ArcGIS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="16ee4-103">Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS</span><span class="sxs-lookup"><span data-stu-id="16ee4-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="16ee4-104">Ebben az oktatóanyagban elsajátíthatja az Azure Active Directoryval (Azure AD) integrálása ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="16ee4-104">In this tutorial, you learn how to integrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="16ee4-105">ArcGIS Online integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="16ee4-105">Integrating ArcGIS Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="16ee4-106">Szabályozhatja, aki hozzáfér ArcGIS Online Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="16ee4-106">You can control in Azure AD who has access to ArcGIS Online</span></span>
- <span data-ttu-id="16ee4-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett ArcGIS online (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="16ee4-107">You can enable your users to automatically get signed-on to ArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="16ee4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="16ee4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="16ee4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="16ee4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="16ee4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="16ee4-110">Prerequisites</span></span>

<span data-ttu-id="16ee4-111">Az Azure AD-integráció konfigurálása a ArcGIS Online, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="16ee4-111">To configure Azure AD integration with ArcGIS Online, you need the following items:</span></span>

- <span data-ttu-id="16ee4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="16ee4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="16ee4-113">Egy ArcGIS Online egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="16ee4-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="16ee4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="16ee4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="16ee4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="16ee4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="16ee4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="16ee4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="16ee4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="16ee4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="16ee4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="16ee4-118">Scenario description</span></span>
<span data-ttu-id="16ee4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="16ee4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="16ee4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="16ee4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="16ee4-121">A gyűjteményből ArcGIS Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="16ee4-121">Adding ArcGIS Online from the gallery</span></span>
2. <span data-ttu-id="16ee4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="16ee4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-the-gallery"></a><span data-ttu-id="16ee4-123">A gyűjteményből ArcGIS Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="16ee4-123">Adding ArcGIS Online from the gallery</span></span>
<span data-ttu-id="16ee4-124">Az Azure AD integrálása a ArcGIS Online konfigurálásához kell hozzáadnia ArcGIS Online a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="16ee4-124">To configure the integration of ArcGIS Online into Azure AD, you need to add ArcGIS Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="16ee4-125">**Adja hozzá a ArcGIS Online a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16ee4-125">**To add ArcGIS Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="16ee4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="16ee4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="16ee4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="16ee4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="16ee4-131">Kattintson a **új alkalmazás** gombra az új alkalmazás hozzáadása párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="16ee4-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="16ee4-133">Írja be a keresőmezőbe, **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-133">In the search box, type **ArcGIS Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="16ee4-135">Az eredmények panelen válassza ki a **ArcGIS Online**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="16ee4-135">In the results panel, select **ArcGIS Online**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="16ee4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="16ee4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="16ee4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés ArcGIS Online "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="16ee4-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="16ee4-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó ArcGIS online Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="16ee4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ArcGIS Online is to a user in Azure AD.</span></span> <span data-ttu-id="16ee4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a ArcGIS Online közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="16ee4-140">In other words, a link relationship between an Azure AD user and the related user in ArcGIS Online needs to be established.</span></span>

<span data-ttu-id="16ee4-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** ArcGIS online.</span><span class="sxs-lookup"><span data-stu-id="16ee4-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="16ee4-142">Az Azure AD egyszeri bejelentkezést a ArcGIS Online tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="16ee4-142">To configure and test Azure AD single sign-on with ArcGIS Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="16ee4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="16ee4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="16ee4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="16ee4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="16ee4-145">**[Egy ArcGIS Online tesztfelhasználó létrehozása](#creating-an-arcgis-online-test-user)**  - való egy megfelelője a Britta Simon ArcGIS Online, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="16ee4-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - to have a counterpart of Britta Simon in ArcGIS Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="16ee4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="16ee4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="16ee4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="16ee4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="16ee4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="16ee4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="16ee4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ArcGIS Online alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="16ee4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="16ee4-150">**Az Azure AD egyszeri bejelentkezést a ArcGIS Online megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16ee4-150">**To configure Azure AD single sign-on with ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="16ee4-151">Az Azure portálon a a **ArcGIS Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-151">In the Azure portal, on the **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="16ee4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="16ee4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="16ee4-155">Az a **ArcGIS Online tartomány és az URL-címek** csoportjában hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="16ee4-155">On the **ArcGIS Online Domain and URLs** section, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="16ee4-157">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="16ee4-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="16ee4-158">Ez az érték nem a valós.</span><span class="sxs-lookup"><span data-stu-id="16ee4-158">This value is not the real.</span></span> <span data-ttu-id="16ee4-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="16ee4-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="16ee4-160">Ügyfél [ArcGIS Online ügyfél-támogatási csoport](http://support.esri.com/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="16ee4-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) to get this value.</span></span> 

4. <span data-ttu-id="16ee4-161">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="16ee4-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="16ee4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="16ee4-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="16ee4-165">Egy másik webes böngészőablakban jelentkezzen be a ArcGIS vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="16ee4-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="16ee4-166">Kattintson a **beállítások szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="16ee4-167">![Beállítások szerkesztése](./media/active-directory-saas-arcgis-tutorial/ic784742.png "beállításainak szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="16ee4-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="16ee4-168">Kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-168">Click **Security**.</span></span>

    <span data-ttu-id="16ee4-169">![Biztonsági](./media/active-directory-saas-arcgis-tutorial/ic784743.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="16ee4-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="16ee4-170">A **vállalati bejelentkezések**, kattintson a **IDENTITÁSSZOLGÁLTATÓ beállítása**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="16ee4-171">![Vállalati bejelentkezések](./media/active-directory-saas-arcgis-tutorial/ic784744.png "vállalati bejelentkezések")</span><span class="sxs-lookup"><span data-stu-id="16ee4-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="16ee4-172">Az a **identitásszolgáltató beállítása** konfigurációs lapján tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="16ee4-172">On the **Set Identity Provider** configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="16ee4-173">![Identitásszolgáltató beállítása](./media/active-directory-saas-arcgis-tutorial/ic784745.png "identitásszolgáltató beállítása")</span><span class="sxs-lookup"><span data-stu-id="16ee4-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="16ee4-174">a.</span><span class="sxs-lookup"><span data-stu-id="16ee4-174">a.</span></span> <span data-ttu-id="16ee4-175">Az a **neve** szövegmező, írja be a szervezet nevét.</span><span class="sxs-lookup"><span data-stu-id="16ee4-175">In the **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="16ee4-176">b.</span><span class="sxs-lookup"><span data-stu-id="16ee4-176">b.</span></span> <span data-ttu-id="16ee4-177">A **használatával a vállalati identitásszolgáltató tartozó metaadatok lesznek megadott**, jelölje be **A fájl**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-177">For **Metadata for the Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="16ee4-178">c.</span><span class="sxs-lookup"><span data-stu-id="16ee4-178">c.</span></span> <span data-ttu-id="16ee4-179">A letöltött metaadat-fájl feltöltése, kattintson a **fájl kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-179">To upload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="16ee4-180">d.</span><span class="sxs-lookup"><span data-stu-id="16ee4-180">d.</span></span> <span data-ttu-id="16ee4-181">Kattintson a **SET IDENTITÁSSZOLGÁLTATÓ**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="16ee4-182">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="16ee4-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="16ee4-183">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="16ee4-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="16ee4-184">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="16ee4-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="16ee4-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ee4-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="16ee4-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="16ee4-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="16ee4-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16ee4-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="16ee4-189">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="16ee4-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="16ee4-191">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="16ee4-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="16ee4-193">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16ee4-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="16ee4-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="16ee4-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="16ee4-197">a.</span><span class="sxs-lookup"><span data-stu-id="16ee4-197">a.</span></span> <span data-ttu-id="16ee4-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="16ee4-199">b.</span><span class="sxs-lookup"><span data-stu-id="16ee4-199">b.</span></span> <span data-ttu-id="16ee4-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16ee4-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="16ee4-201">c.</span><span class="sxs-lookup"><span data-stu-id="16ee4-201">c.</span></span> <span data-ttu-id="16ee4-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="16ee4-203">d.</span><span class="sxs-lookup"><span data-stu-id="16ee4-203">d.</span></span> <span data-ttu-id="16ee4-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="16ee4-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="16ee4-205">Egy ArcGIS Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ee4-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="16ee4-206">Ahhoz, hogy az Azure AD-felhasználók ArcGIS Online bejelentkezni, akkor ki kell építenie ArcGIS Online be.</span><span class="sxs-lookup"><span data-stu-id="16ee4-206">In order to enable Azure AD users to log into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="16ee4-207">ArcGIS Online esetében kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="16ee4-207">In the case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="16ee4-208">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16ee4-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="16ee4-209">Jelentkezzen be a **ArcGIS** bérlő.</span><span class="sxs-lookup"><span data-stu-id="16ee4-209">Log in to your **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="16ee4-210">Kattintson a **tagok MEGHÍVÁSA**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="16ee4-211">![Tagok meghívása](./media/active-directory-saas-arcgis-tutorial/ic784747.png "tagok meghívása")</span><span class="sxs-lookup"><span data-stu-id="16ee4-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="16ee4-212">Válassza ki **tagok automatikus hozzáadása nélkül e-mail**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="16ee4-213">![Tagok hozzáadása automatikusan](./media/active-directory-saas-arcgis-tutorial/ic784748.png "tagok automatikus hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="16ee4-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="16ee4-214">Az a **tagok** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="16ee4-214">On the **Members** dialog page, perform the following steps:</span></span>
   
     <span data-ttu-id="16ee4-215">![Adja hozzá, és tekintse át](./media/active-directory-saas-arcgis-tutorial/ic784749.png "hozzáadása, és nézze meg")</span><span class="sxs-lookup"><span data-stu-id="16ee4-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="16ee4-216">a.</span><span class="sxs-lookup"><span data-stu-id="16ee4-216">a.</span></span> <span data-ttu-id="16ee4-217">Adja meg a **E-mail**, **Utónév**, és **Vezetéknév** egy érvényes AAD-fióknevet, amelyet a rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="16ee4-217">Enter the **Email**, **First Name**, and **Last Name** of a valid AAD account you want to provision.</span></span>
  
     <span data-ttu-id="16ee4-218">b.</span><span class="sxs-lookup"><span data-stu-id="16ee4-218">b.</span></span> <span data-ttu-id="16ee4-219">Kattintson a **hozzáadása, és NÉZZE meg**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="16ee4-220">Tekintse át az adatokat adta-e, és kattintson **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-220">Review the data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="16ee4-221">![Tag hozzáadása](./media/active-directory-saas-arcgis-tutorial/ic784750.png "tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="16ee4-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="16ee4-222">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="16ee4-222">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="16ee4-223">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="16ee4-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="16ee4-224">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó ArcGIS online-hoz való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="16ee4-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ArcGIS Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="16ee4-226">**Britta Simon hozzárendelése ArcGIS Online, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16ee4-226">**To assign Britta Simon to ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="16ee4-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="16ee4-229">Az alkalmazások listában válassza ki a **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-229">In the applications list, select **ArcGIS Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="16ee4-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="16ee4-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="16ee4-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="16ee4-233">Click **Add** button.</span></span> <span data-ttu-id="16ee4-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16ee4-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="16ee4-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="16ee4-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="16ee4-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16ee4-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="16ee4-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16ee4-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="16ee4-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="16ee4-239">Testing single sign-on</span></span>

<span data-ttu-id="16ee4-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="16ee4-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="16ee4-241">Ha a hozzáférési panelen ArcGIS Online csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ArcGIS Online alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="16ee4-241">When you click the ArcGIS Online tile in the Access Panel, you should get automatically signed-on to your ArcGIS Online application.</span></span>
<span data-ttu-id="16ee4-242">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="16ee4-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16ee4-243">További források</span><span class="sxs-lookup"><span data-stu-id="16ee4-243">Additional resources</span></span>

* [<span data-ttu-id="16ee4-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="16ee4-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="16ee4-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="16ee4-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

