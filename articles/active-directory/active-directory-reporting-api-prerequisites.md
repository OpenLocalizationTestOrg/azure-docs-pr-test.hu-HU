---
title: "Előfeltételek az Azure AD reporting API eléréséhez. | Microsoft Docs"
description: "További tudnivalók az Azure AD reporting API eléréséhez Előfeltételek"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="c8876-104">Az Azure AD reporting API eléréséhez Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8876-104">Prerequisites to access the Azure AD reporting API</span></span>
<span data-ttu-id="c8876-105">A [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) keresztül REST-alapú API-készlet programozott hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="c8876-105">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="c8876-106">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="c8876-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="c8876-107">A jelentéskészítési API által használt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) engedélyezéséhez a web API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c8876-107">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="c8876-108">Készítse elő a reporting API a hozzáférést, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="c8876-108">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="c8876-109">Alkalmazás létrehozása az Azure AD-bérlőben</span><span class="sxs-lookup"><span data-stu-id="c8876-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="c8876-110">Az alkalmazás az Azure AD-adatok eléréséhez szükséges engedélyeket adja meg</span><span class="sxs-lookup"><span data-stu-id="c8876-110">Grant the application appropriate permissions to access the Azure AD data</span></span>
3. <span data-ttu-id="c8876-111">Konfigurációs beállítások összegyűjtése a címtárban</span><span class="sxs-lookup"><span data-stu-id="c8876-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="c8876-112">Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c8876-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="c8876-113">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8876-113">Create an Azure AD application</span></span>
<span data-ttu-id="c8876-114">A címtár az Azure AD reporting API eléréséhez konfigurálásához be kell jelentkeznie Azure-előfizetéshez rendszergazdai fiókkal, amely tagja is az Azure AD-bérlő globális rendszergazdája directory szerepkörének a klasszikus Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="c8876-114">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure classic portal with an Azure subscription administrator account that is also a member of the Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8876-115">Ehhez hasonló "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így az alkalmazás azonosítója/titkos hitelesítő adatok a biztonságos ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="c8876-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="c8876-116">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c8876-116">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="c8876-118">Az a **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="c8876-118">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="c8876-119">Kattintson a felső menüben **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8876-119">In the menu on the top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="c8876-121">Kattintson az alsó sáv **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c8876-121">On the bottom bar, click **Add**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="c8876-123">Az a **mi történjen a teendő?** párbeszédpanel, kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c8876-123">On the **What do you want to do?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="c8876-125">Az a **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c8876-125">On the **Tell us about your application** dialog, perform the following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="c8876-127">a.</span><span class="sxs-lookup"><span data-stu-id="c8876-127">a.</span></span> <span data-ttu-id="c8876-128">Az a **neve** szövegmező, írja be nevét (pl.: Reporting API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="c8876-128">In the **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="c8876-129">b.</span><span class="sxs-lookup"><span data-stu-id="c8876-129">b.</span></span> <span data-ttu-id="c8876-130">Válassza ki **webalkalmazás és/vagy webes API-t**.</span><span class="sxs-lookup"><span data-stu-id="c8876-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="c8876-131">c.</span><span class="sxs-lookup"><span data-stu-id="c8876-131">c.</span></span> <span data-ttu-id="c8876-132">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8876-132">Click **Next**.</span></span>
7. <span data-ttu-id="c8876-133">Az a **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c8876-133">On the **App properties** dialog, perform the following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="c8876-135">a.</span><span class="sxs-lookup"><span data-stu-id="c8876-135">a.</span></span> <span data-ttu-id="c8876-136">Az a **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c8876-136">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="c8876-137">b.</span><span class="sxs-lookup"><span data-stu-id="c8876-137">b.</span></span> <span data-ttu-id="c8876-138">Az a **App ID URI** szövegmezőhöz típus ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="c8876-138">In the **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="c8876-139">c.</span><span class="sxs-lookup"><span data-stu-id="c8876-139">c.</span></span> <span data-ttu-id="c8876-140">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8876-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="c8876-141">Adja meg az alkalmazás számára az API-val</span><span class="sxs-lookup"><span data-stu-id="c8876-141">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="c8876-142">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c8876-142">In the [Azure classic portal](https://manage.windowsazure.com/), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="c8876-144">Az a **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="c8876-144">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="c8876-145">Kattintson a felső menüben **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8876-145">In the menu on the top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="c8876-147">Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c8876-147">In the applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="c8876-149">Kattintson a felső menüben **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="c8876-149">In the menu on the top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="c8876-151">Az a **egyéb alkalmazások engedélyei** szakaszban, a a **Azure Active Directory** erőforrás, kattintson a **Alkalmazásengedélyek** legördülő listából válassza ki, és válassza ki **Címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="c8876-151">In the **Permissions to other applications** section, for the **Azure Active Directory** resource, click the **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="c8876-153">Kattintson az alsó sáv **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c8876-153">On the bottom bar, click **Save**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="c8876-155">Konfigurációs beállítások összegyűjtése a címtárban</span><span class="sxs-lookup"><span data-stu-id="c8876-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="c8876-156">Ez a szakasz bemutatja, hogyan az a Directoryból olvassa ki az alábbi beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c8876-156">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="c8876-157">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="c8876-157">Domain name</span></span>
* <span data-ttu-id="c8876-158">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="c8876-158">Client ID</span></span>
* <span data-ttu-id="c8876-159">Titkos ügyfélkulcs</span><span class="sxs-lookup"><span data-stu-id="c8876-159">Client secret</span></span>

<span data-ttu-id="c8876-160">A jelentéskészítési API hívásainak konfigurálásakor kell ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c8876-160">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="c8876-161">A tartománynév beolvasása</span><span class="sxs-lookup"><span data-stu-id="c8876-161">Get your domain name</span></span>
1. <span data-ttu-id="c8876-162">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c8876-162">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="c8876-164">Az a **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="c8876-164">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="c8876-165">Kattintson a felső menüben **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="c8876-165">In the menu on the top, click **Domains**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="c8876-167">Az a **tartománynév** oszlop, másolja a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="c8876-167">In the **Domain Name** column, copy your domain name.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a><span data-ttu-id="c8876-169">Az alkalmazás ügyfél-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="c8876-169">Get the application's client ID</span></span>
1. <span data-ttu-id="c8876-170">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c8876-170">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="c8876-172">Az a **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="c8876-172">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="c8876-173">Kattintson a felső menüben **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8876-173">In the menu on the top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="c8876-175">Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c8876-175">In the applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="c8876-177">Kattintson a felső menüben **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="c8876-177">In the menu on the top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="c8876-179">Másolás a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="c8876-179">Copy your **Client ID**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a><span data-ttu-id="c8876-181">Az alkalmazás ügyfél titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="c8876-181">Get the application's client secret</span></span>
<span data-ttu-id="c8876-182">Ahhoz, hogy az alkalmazás ügyfélkulcs, meg kell hozzon létre egy új kulcsot, és mentse az új kulcs mentése, mert nincs lehetőség a később többé lekérje ezt az értéket, az értékét.</span><span class="sxs-lookup"><span data-stu-id="c8876-182">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

1. <span data-ttu-id="c8876-183">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c8876-183">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="c8876-185">Az a **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="c8876-185">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="c8876-186">Kattintson a felső menüben **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8876-186">In the menu on the top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="c8876-188">Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c8876-188">In the applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="c8876-190">Kattintson a felső menüben **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="c8876-190">In the menu on the top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="c8876-192">Az a **kulcsok** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c8876-192">In the **Keys** section, perform the following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="c8876-194">a.</span><span class="sxs-lookup"><span data-stu-id="c8876-194">a.</span></span> <span data-ttu-id="c8876-195">Időtartam listájából válasszon egy időtartamot</span><span class="sxs-lookup"><span data-stu-id="c8876-195">From the duration list, select a duration</span></span>
   
    <span data-ttu-id="c8876-196">b.</span><span class="sxs-lookup"><span data-stu-id="c8876-196">b.</span></span> <span data-ttu-id="c8876-197">Kattintson az alsó sáv **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c8876-197">On the bottom bar, click **Save**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="c8876-199">c.</span><span class="sxs-lookup"><span data-stu-id="c8876-199">c.</span></span> <span data-ttu-id="c8876-200">Másolja a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="c8876-200">Copy the key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8876-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8876-201">Next Steps</span></span>
* <span data-ttu-id="c8876-202">Szeretné az Azure AD reporting API-val programozott módon érheti el az adatait?</span><span class="sxs-lookup"><span data-stu-id="c8876-202">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="c8876-203">Tekintse meg [Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c8876-203">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="c8876-204">Ha meg szeretne többet megtudni az Azure Active Directory reporting, tekintse meg a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="c8876-204">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

