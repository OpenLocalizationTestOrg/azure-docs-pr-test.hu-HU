---
title: "Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az IBM Kenexa felmérés vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c276c23288292a1c54dd9d57177d5072b90c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a><span data-ttu-id="9730d-103">Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati</span><span class="sxs-lookup"><span data-stu-id="9730d-103">Tutorial: Azure Active Directory integration with IBM Kenexa Survey Enterprise</span></span>

<span data-ttu-id="9730d-104">Ebben az oktatóanyagban elsajátíthatja IBM Kenexa felmérés vállalati integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9730d-104">In this tutorial, you learn how to integrate IBM Kenexa Survey Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9730d-105">IBM Kenexa felmérés vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9730d-105">Integrating IBM Kenexa Survey Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9730d-106">Az Azure AD, aki hozzáfér az IBM Kenexa felmérés vállalati szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="9730d-106">You can control in Azure AD who has access to IBM Kenexa Survey Enterprise.</span></span>
- <span data-ttu-id="9730d-107">Engedélyezheti a felhasználók automatikusan jelentkezhetnek be IBM Kenexa felmérés vállalati az Azure AD-fiókok egyszeri bejelentkezés (SSO) használatával.</span><span class="sxs-lookup"><span data-stu-id="9730d-107">You can enable your users to automatically sign in to IBM Kenexa Survey Enterprise by using single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9730d-108">A fiók egyetlen központi helyen kezelheti: az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9730d-108">You can manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="9730d-109">Ha azt szeretné, az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazás integrációt, tudnia további információért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9730d-109">If you want to know more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9730d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9730d-110">Prerequisites</span></span>

<span data-ttu-id="9730d-111">Az Azure AD-integráció konfigurálása a IBM Kenexa felmérés vállalati, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9730d-111">To configure Azure AD integration with IBM Kenexa Survey Enterprise, you need the following items:</span></span>

- <span data-ttu-id="9730d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9730d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9730d-113">Az IBM Kenexa felmérés vállalati SSO-kompatibilis előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="9730d-113">An IBM Kenexa Survey Enterprise SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9730d-114">Ebben az oktatóanyagban tesztelésekor a lépéseket, azt javasoljuk, hogy nem használ egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9730d-114">When you test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="9730d-115">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="9730d-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9730d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9730d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9730d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9730d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9730d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9730d-118">Scenario description</span></span>
<span data-ttu-id="9730d-119">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="9730d-119">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="9730d-120">Az oktatóanyag ismertetett forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9730d-120">The scenario outlined in the tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="9730d-121">IBM Kenexa felmérés vállalati hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9730d-121">Adding IBM Kenexa Survey Enterprise from the gallery</span></span>
* <span data-ttu-id="9730d-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9730d-122">Configuring and testing Azure AD SSO</span></span>

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a><span data-ttu-id="9730d-123">IBM Kenexa felmérés vállalati hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9730d-123">Add IBM Kenexa Survey Enterprise from the gallery</span></span>
<span data-ttu-id="9730d-124">Az Azure AD integrálása a IBM Kenexa felmérés vállalati megadásához vegye fel IBM Kenexa felmérés vállalati a gyűjteményből a felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="9730d-124">To configure the integration of IBM Kenexa Survey Enterprise into Azure AD, add IBM Kenexa Survey Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9730d-125">A gyűjteményből IBM Kenexa felmérés vállalati hozzáadásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9730d-125">To add IBM Kenexa Survey Enterprise from the gallery, do the following:</span></span>

1. <span data-ttu-id="9730d-126">Az a [Azure-portálon](https://portal.azure.com), a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-126">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Active Directory** button.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="9730d-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9730d-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="9730d-130">Hozzáadhat egy alkalmazást, kattintson a **új alkalmazás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-130">To add an application, click the **New application** button.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="9730d-132">Írja be a keresőmezőbe, **IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="9730d-132">In the search box, type **IBM Kenexa Survey Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. <span data-ttu-id="9730d-134">Az eredmények listájában válassza **IBM Kenexa felmérés vállalati**, majd kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9730d-134">In the results list, select **IBM Kenexa Survey Enterprise**, and then click the **Add** button to add the application.</span></span>

    ![IBM Kenexa felmérés vállalati az eredménylistában](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9730d-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9730d-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9730d-137">Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO IBM Kenexa felmérés vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="9730d-137">In this section, you configure and test Azure AD SSO with IBM Kenexa Survey Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9730d-138">Az egyszeri bejelentkezés működjön az Azure AD az Azure ad-ben az IBM Kenexa felmérés vállalati felhasználó megfelelőjére azonosítania kell.</span><span class="sxs-lookup"><span data-stu-id="9730d-138">For SSO to work, Azure AD needs to identify the IBM Kenexa Survey Enterprise user counterpart in Azure AD.</span></span> <span data-ttu-id="9730d-139">Ez azt jelenti az Azure AD kapcsolatot kell létesítenie egy hivatkozás egy Azure AD-felhasználó és a kapcsolódó felhasználó között IBM Kenexa felmérés vállalati.</span><span class="sxs-lookup"><span data-stu-id="9730d-139">In other words, Azure AD must establish a link relationship between an Azure AD user and a related user in IBM Kenexa Survey Enterprise.</span></span>

<span data-ttu-id="9730d-140">A hivatkozás kapcsolatot létesíteni, rendelje az értékét a **felhasználónév** IBM Kenexa felmérés vállalati értékeként a **felhasználónév** az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9730d-140">To establish the link relationship, assign the value of the **user name** in IBM Kenexa Survey Enterprise as the value of the **Username** in Azure AD.</span></span>

<span data-ttu-id="9730d-141">IBM Kenexa felmérés vállalati Azure AD egyszeri bejelentkezés tesztelése és konfigurálása, végezze el az építőelemeket, a következő két szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="9730d-141">To configure and test Azure AD SSO with IBM Kenexa Survey Enterprise, complete the building blocks in the next two sections.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="9730d-142">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9730d-142">Configure Azure AD SSO</span></span>

<span data-ttu-id="9730d-143">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést az Azure portálon, és egyszeri bejelentkezés konfigurálása az IBM Kenexa felmérés vállalati alkalmazás a következő módon:</span><span class="sxs-lookup"><span data-stu-id="9730d-143">In this section, you enable Azure AD SSO in the Azure portal and configure SSO in your IBM Kenexa Survey Enterprise application by doing the following:</span></span>

1. <span data-ttu-id="9730d-144">Az Azure portálon a a **IBM Kenexa felmérés vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9730d-144">In the Azure portal, on the **IBM Kenexa Survey Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![IBM Kenexa felmérés vállalati konfigurálása egyszeri bejelentkezés hivatkozás][4]

2. <span data-ttu-id="9730d-146">Az a **egyszeri bejelentkezés** párbeszédpanel a **mód** mezőben válassza **SAML-alapú bejelentkezés** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9730d-146">In the **Single sign-on** dialog box, in the **Mode** box, select **SAML-based Sign-on** to enable SSO.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. <span data-ttu-id="9730d-148">Az a **IBM Kenexa felmérés vállalati tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9730d-148">In the **IBM Kenexa Survey Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![IBM Kenexa felmérés vállalati tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    <span data-ttu-id="9730d-150">a.</span><span class="sxs-lookup"><span data-stu-id="9730d-150">a.</span></span> <span data-ttu-id="9730d-151">Az a **azonosító** szövegmezőhöz URL-címet adja meg a következő mintával:`https://surveys.kenexa.com/<companycode>`</span><span class="sxs-lookup"><span data-stu-id="9730d-151">In the **Identifier** textbox, type a URL with the following pattern: `https://surveys.kenexa.com/<companycode>`</span></span>

    <span data-ttu-id="9730d-152">b.</span><span class="sxs-lookup"><span data-stu-id="9730d-152">b.</span></span> <span data-ttu-id="9730d-153">Az a **válasz URL-CÍMEN** szövegmezőhöz URL-címet adja meg a következő mintával:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span><span class="sxs-lookup"><span data-stu-id="9730d-153">In the **Reply URL** textbox, type a URL with the following pattern: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9730d-154">Az előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="9730d-154">The preceding values are not real.</span></span> <span data-ttu-id="9730d-155">A tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9730d-155">Update them with the actual identifier and reply URL.</span></span> <span data-ttu-id="9730d-156">A tényleges értékek beszerzéséhez forduljon a [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="9730d-156">To obtain the actual values, contact the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

4. <span data-ttu-id="9730d-157">A **SAML-aláíró tanúsítványa**, kattintson a **tanúsítvány (Base64)**, és mentse a fájlt a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="9730d-157">Under **SAML Signing Certificate**, click **Certificate (Base64)**, and then save the certificate file to your computer.</span></span>

    ![A tanúsítvány (Base64) letöltési hivatkozását](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    <span data-ttu-id="9730d-159">Az IBM Kenexa felmérés vállalati alkalmazás vár a biztonsági helyességi feltételek Markup Language (SAML) helyességi feltételek fogadásához egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9730d-159">The IBM Kenexa Survey Enterprise application expects to receive the Security Assertions Markup Language (SAML) assertions in a specific format, which requires you to add custom attribute mappings to the configuration of your SAML token attributes.</span></span> <span data-ttu-id="9730d-160">A válaszban a felhasználó-azonosító jogcím értékét meg kell egyeznie az egyszeri bejelentkezési azonosító a Kenexa rendszerben konfigurált.</span><span class="sxs-lookup"><span data-stu-id="9730d-160">The value of the user-identifier claim in the response must match the SSO ID that's configured in the Kenexa system.</span></span> <span data-ttu-id="9730d-161">A megfelelő felhasználói azonosítót a szervezetében, egyszeri Bejelentkezéses Internet Datagram Protocol (IDP) megfeleltetéséhez dolgozni a [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="9730d-161">To map the appropriate user identifier in your organization as SSO Internet Datagram Protocol (IDP), work with the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> 

    <span data-ttu-id="9730d-162">Alapértelmezés szerint az Azure AD a felhasználói azonosító a felhasználó egyszerű felhasználónév (UPN) értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="9730d-162">By default, Azure AD sets the user identifier as the user principal name (UPN) value.</span></span> <span data-ttu-id="9730d-163">Módosíthatja ezt az értéket a **attribútum** lapon, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="9730d-163">You can change this value on the **Attribute** tab, as shown in the following screenshot.</span></span> <span data-ttu-id="9730d-164">Az integráció csak a leképezési befejezése után működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9730d-164">The integration works only after you've completed the mapping correctly.</span></span>
    
    ![A felhasználói attribútumok párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png)   

5. <span data-ttu-id="9730d-166">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-166">Click **Save**.</span></span>

    ![A konfigurálása egyszeri bejelentkezéshez mentési gomb](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9730d-168">Megnyitásához a **bejelentkezés konfigurálása** ablakban, a **IBM Kenexa felmérés vállalati konfiguráció**, kattintson a **konfigurálása IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="9730d-168">To open the **Configure sign-on** window, under **IBM Kenexa Survey Enterprise Configuration**, click **Configure IBM Kenexa Survey Enterprise**.</span></span> 
 
    ![A konfigurálása IBM Kenexa felmérés vállalati hivatkozás](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. <span data-ttu-id="9730d-170">Másolás a **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** közötti értéket a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9730d-170">Copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values from the **Quick Reference** section.</span></span>

8. <span data-ttu-id="9730d-171">Az a **bejelentkezés konfigurálása** ablakban, a **rövid összefoglaló**, másolása a **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** értékeket.</span><span class="sxs-lookup"><span data-stu-id="9730d-171">In the **Configure sign-on** window, under **Quick Reference**, copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values.</span></span>

9. <span data-ttu-id="9730d-172">Egyszeri bejelentkezés konfigurálása a **IBM Kenexa felmérés vállalati** oldalán, küldjön a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** értékeit a a [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="9730d-172">To configure SSO on the **IBM Kenexa Survey Enterprise** side, send the downloaded **Certificate (Base64)**, **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values to the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

> [!TIP]
> <span data-ttu-id="9730d-173">Olvassa el ezeket az utasításokat a tömör verzióját a [Azure-portálon](https://portal.azure.com) közben állítja be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9730d-173">You can refer to a concise version of these instructions in the [Azure portal](https://portal.azure.com) while you are setting up the app.</span></span> <span data-ttu-id="9730d-174">Miután hozzáadta az alkalmazásból a **Active Directory** > **vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapot, és hozzáférhet a beágyazott dokumentáció keresztül a **konfigurációs** szakasz végén.</span><span class="sxs-lookup"><span data-stu-id="9730d-174">After you add the app from the **Active Directory** > **Enterprise Applications** section, simply click the **single sign-on** tab, and then access the embedded documentation through the **Configuration** section at the end.</span></span> <span data-ttu-id="9730d-175">A beágyazott dokumentáció szolgáltatással kapcsolatos további tudnivalókért lásd: [az Azure AD dokumentációjában beágyazott](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="9730d-175">To learn more about the embedded documentation feature, see [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9730d-176">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9730d-176">Create an Azure AD test user</span></span>
<span data-ttu-id="9730d-177">Ez a szakasz az alábbi lépésekkel hoz létre tesztfelhasználó Britta Simon az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="9730d-177">In this section, you create test user Britta Simon in the Azure portal by doing the following:</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

1. <span data-ttu-id="9730d-179">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-179">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9730d-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9730d-181">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9730d-183">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="9730d-183">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9730d-185">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9730d-185">In the **User** dialog box, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9730d-187">a.</span><span class="sxs-lookup"><span data-stu-id="9730d-187">a.</span></span> <span data-ttu-id="9730d-188">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9730d-188">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9730d-189">b.</span><span class="sxs-lookup"><span data-stu-id="9730d-189">b.</span></span> <span data-ttu-id="9730d-190">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9730d-190">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="9730d-191">c.</span><span class="sxs-lookup"><span data-stu-id="9730d-191">c.</span></span> <span data-ttu-id="9730d-192">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9730d-192">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="9730d-193">d.</span><span class="sxs-lookup"><span data-stu-id="9730d-193">d.</span></span> <span data-ttu-id="9730d-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-194">Click **Create**.</span></span>
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a><span data-ttu-id="9730d-195">Az IBM Kenexa felmérés vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9730d-195">Create an IBM Kenexa Survey Enterprise test user</span></span>

<span data-ttu-id="9730d-196">Ebben a szakaszban Britta Simon meghívta IBM Kenexa felmérés vállalati felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9730d-196">In this section, you create a user called Britta Simon in IBM Kenexa Survey Enterprise.</span></span> 

<span data-ttu-id="9730d-197">A felhasználók létrehozásához az IBM Kenexa felmérés Enterprise rendszerben, és az egyszeri bejelentkezési azonosító hozzárendelését a számukra, használhatja a [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="9730d-197">To create users in the IBM Kenexa Survey Enterprise system and map the SSO ID for them, you can work with the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> <span data-ttu-id="9730d-198">Ezt az egyszeri bejelentkezési azonosító értéket is kell rendelni a felhasználói azonosító értékét az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9730d-198">This SSO ID value should also be mapped to the user identifier value from Azure AD.</span></span> <span data-ttu-id="9730d-199">Módosíthatja az alapértelmezett beállítás a **attribútum** fülre.</span><span class="sxs-lookup"><span data-stu-id="9730d-199">You can change this default setting on the **Attribute** tab.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9730d-200">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9730d-200">Assign the Azure AD test user</span></span>

<span data-ttu-id="9730d-201">Ebben a szakaszban engedélyezze felhasználói Britta Simon által biztosított hozzáférés IBM Kenexa felmérés vállalati Azure SSO használandó.</span><span class="sxs-lookup"><span data-stu-id="9730d-201">In this section, you enable user Britta Simon to use Azure SSO by granting access to IBM Kenexa Survey Enterprise.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="9730d-203">Felhasználó Britta Simon hozzárendelése IBM Kenexa felmérés vállalati, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9730d-203">To assign user Britta Simon to IBM Kenexa Survey Enterprise, do the following:</span></span>

1. <span data-ttu-id="9730d-204">Az Azure portálon, nyissa meg a **alkalmazások** nézet, keresse fel a **Directory** nézetben jelölje ki **vállalati alkalmazások**, és kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9730d-204">In the Azure portal, open the **Applications** view, go to the **Directory** view, select **Enterprise applications**, and then click **All applications**.</span></span>

    ![A "Vállalati alkalmazások" és "Összes alkalmazás" hivatkozások][201] 

2. <span data-ttu-id="9730d-206">Az a **alkalmazások** listáról válassza ki **IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="9730d-206">In the **Applications** list, select **IBM Kenexa Survey Enterprise**.</span></span>

    ![Az alkalmazások listáját az IBM Kenexa felmérés vállalati hivatkozás](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. <span data-ttu-id="9730d-208">Kattintson a bal oldali ablaktáblában **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9730d-208">In the left pane, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="9730d-210">Kattintson a **Hozzáadás** gombra, majd a a **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9730d-210">Click the **Add** button and then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="9730d-212">Az a **felhasználók és csoportok** párbeszédpanel a **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9730d-212">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="9730d-213">Az a **felhasználók és csoportok** párbeszédpanel, kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-213">In the **Users and groups** dialog box, click the **Select** button.</span></span>

7. <span data-ttu-id="9730d-214">Az a **hozzáadása hozzárendelés** párbeszédpanel, kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9730d-214">In the **Add Assignment** dialog box, click the **Assign** button.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9730d-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9730d-215">Test single sign-on</span></span>

<span data-ttu-id="9730d-216">Ebben a szakaszban az Azure AD SSO konfigurációját a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9730d-216">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="9730d-217">Amikor rákattint az **IBM Kenexa felmérés vállalati** csempére a hozzáférési panelen, meg kell lennie automatikusan bejelentkeztetjük az IBM Kenexa felmérés vállalati alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9730d-217">When you click the **IBM Kenexa Survey Enterprise** tile in the Access Panel, you should be automatically signed in to your IBM Kenexa Survey Enterprise application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9730d-218">További források</span><span class="sxs-lookup"><span data-stu-id="9730d-218">Additional resources</span></span>

* [<span data-ttu-id="9730d-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9730d-219">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9730d-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9730d-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
