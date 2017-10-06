---
title: "aaaPrerequisites tooaccess hello Azure AD jelentéskészítési API. | Microsoft Docs"
description: "További tudnivalók: hello Előfeltételek tooaccess hello Azure AD jelentéskészítési API"
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
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="32ad1-104">Előfeltételek tooaccess hello Azure AD jelentéskészítési API</span><span class="sxs-lookup"><span data-stu-id="32ad1-104">Prerequisites tooaccess hello Azure AD reporting API</span></span>
<span data-ttu-id="32ad1-105">Hello [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) REST-alapú API-készlet toohello adatokat programozott hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="32ad1-105">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="32ad1-106">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="32ad1-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="32ad1-107">a reporting API által használt hello [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize hozzáférés toohello webes API-k.</span><span class="sxs-lookup"><span data-stu-id="32ad1-107">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="32ad1-108">tooprepare a reporting API access toohello, kell:</span><span class="sxs-lookup"><span data-stu-id="32ad1-108">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="32ad1-109">Alkalmazás létrehozása az Azure AD-bérlőben</span><span class="sxs-lookup"><span data-stu-id="32ad1-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="32ad1-110">Támogatás hello alkalmazás megfelelő engedélyekkel tooaccess hello Azure AD-adatok</span><span class="sxs-lookup"><span data-stu-id="32ad1-110">Grant hello application appropriate permissions tooaccess hello Azure AD data</span></span>
3. <span data-ttu-id="32ad1-111">Konfigurációs beállítások összegyűjtése a címtárban</span><span class="sxs-lookup"><span data-stu-id="32ad1-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="32ad1-112">Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="32ad1-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="32ad1-113">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="32ad1-113">Create an Azure AD application</span></span>
<span data-ttu-id="32ad1-114">tooconfigure a directory tooaccess hello Azure AD jelentéskészítési API, be kell jelentkeznie a toohello Azure-előfizetéshez rendszergazdai fiókkal, amely tagja is hello globális rendszergazda directory szerepkör az Azure AD-bérlő a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="32ad1-114">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure classic portal with an Azure subscription administrator account that is also a member of hello Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32ad1-115">A "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így Felhívjuk biztonságos meg arról, hogy tookeep hello alkalmazás azonosítója/titkos hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="32ad1-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="32ad1-116">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-116">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="32ad1-118">A hello **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="32ad1-118">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="32ad1-119">Hello hello felső menüben kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-119">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="32ad1-121">Az alsó hello menüsávon kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-121">On hello bottom bar, click **Add**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="32ad1-123">A hello **miről szeretne toodo?** párbeszédpanel, kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-123">On hello **What do you want toodo?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="32ad1-125">A hello **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="32ad1-125">On hello **Tell us about your application** dialog, perform hello following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="32ad1-127">a.</span><span class="sxs-lookup"><span data-stu-id="32ad1-127">a.</span></span> <span data-ttu-id="32ad1-128">A hello **neve** szövegmező, írja be nevét (pl.: Reporting API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="32ad1-128">In hello **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="32ad1-129">b.</span><span class="sxs-lookup"><span data-stu-id="32ad1-129">b.</span></span> <span data-ttu-id="32ad1-130">Válassza ki **webalkalmazás és/vagy webes API-t**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="32ad1-131">c.</span><span class="sxs-lookup"><span data-stu-id="32ad1-131">c.</span></span> <span data-ttu-id="32ad1-132">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="32ad1-132">Click **Next**.</span></span>
7. <span data-ttu-id="32ad1-133">A hello **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="32ad1-133">On hello **App properties** dialog, perform hello following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="32ad1-135">a.</span><span class="sxs-lookup"><span data-stu-id="32ad1-135">a.</span></span> <span data-ttu-id="32ad1-136">A hello **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="32ad1-136">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="32ad1-137">b.</span><span class="sxs-lookup"><span data-stu-id="32ad1-137">b.</span></span> <span data-ttu-id="32ad1-138">A hello **App ID URI** szövegmezőhöz típus ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="32ad1-138">In hello **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="32ad1-139">c.</span><span class="sxs-lookup"><span data-stu-id="32ad1-139">c.</span></span> <span data-ttu-id="32ad1-140">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="32ad1-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="32ad1-141">Adja meg az alkalmazás engedélyt toouse hello API</span><span class="sxs-lookup"><span data-stu-id="32ad1-141">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="32ad1-142">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-142">In hello [Azure classic portal](https://manage.windowsazure.com/), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="32ad1-144">A hello **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="32ad1-144">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="32ad1-145">Hello hello felső menüben kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-145">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="32ad1-147">Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="32ad1-147">In hello applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="32ad1-149">Hello hello felső menüben kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-149">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="32ad1-151">A hello **engedélyek tooother alkalmazások** szakasz hello **Azure Active Directory** erőforrás, hello kattintson **Alkalmazásengedélyek** legördülő listában, majd Válassza ki **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-151">In hello **Permissions tooother applications** section, for hello **Azure Active Directory** resource, click hello **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="32ad1-153">Az alsó hello menüsávon kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-153">On hello bottom bar, click **Save**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="32ad1-155">Konfigurációs beállítások összegyűjtése a címtárban</span><span class="sxs-lookup"><span data-stu-id="32ad1-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="32ad1-156">Ez a szakasz bemutatja, hogy a címtárban lévő beállítások következő tooget hello:</span><span class="sxs-lookup"><span data-stu-id="32ad1-156">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="32ad1-157">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="32ad1-157">Domain name</span></span>
* <span data-ttu-id="32ad1-158">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="32ad1-158">Client ID</span></span>
* <span data-ttu-id="32ad1-159">Titkos ügyfélkulcs</span><span class="sxs-lookup"><span data-stu-id="32ad1-159">Client secret</span></span>

<span data-ttu-id="32ad1-160">Hívások toohello jelentéskészítési API konfigurálásakor kell ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="32ad1-160">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="32ad1-161">A tartománynév beolvasása</span><span class="sxs-lookup"><span data-stu-id="32ad1-161">Get your domain name</span></span>
1. <span data-ttu-id="32ad1-162">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-162">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="32ad1-164">A hello **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="32ad1-164">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="32ad1-165">Hello hello felső menüben kattintson a **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-165">In hello menu on hello top, click **Domains**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="32ad1-167">A hello **tartománynév** oszlop, másolja a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="32ad1-167">In hello **Domain Name** column, copy your domain name.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a><span data-ttu-id="32ad1-169">Hello alkalmazás ügyfél-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="32ad1-169">Get hello application's client ID</span></span>
1. <span data-ttu-id="32ad1-170">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-170">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="32ad1-172">A hello **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="32ad1-172">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="32ad1-173">Hello hello felső menüben kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-173">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="32ad1-175">Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="32ad1-175">In hello applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="32ad1-177">Hello hello felső menüben kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-177">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="32ad1-179">Másolás a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-179">Copy your **Client ID**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a><span data-ttu-id="32ad1-181">Szeretne hello alkalmazás ügyfélkulcsot</span><span class="sxs-lookup"><span data-stu-id="32ad1-181">Get hello application's client secret</span></span>
<span data-ttu-id="32ad1-182">tooget az alkalmazás ügyfél titkos, meg kell toocreate egy új kulcsot, és mentse után hello új kulcs mentése, mert már nem lehetséges tooretrieve értéke ezt az értéket később már.</span><span class="sxs-lookup"><span data-stu-id="32ad1-182">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

1. <span data-ttu-id="32ad1-183">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-183">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="32ad1-185">A hello **active directory** listára, válassza ki a címtárban.</span><span class="sxs-lookup"><span data-stu-id="32ad1-185">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="32ad1-186">Hello hello felső menüben kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-186">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="32ad1-188">Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="32ad1-188">In hello applications list, select your newly created application.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="32ad1-190">Hello hello felső menüben kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-190">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="32ad1-192">A hello **kulcsok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="32ad1-192">In hello **Keys** section, perform hello following steps:</span></span> 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="32ad1-194">a.</span><span class="sxs-lookup"><span data-stu-id="32ad1-194">a.</span></span> <span data-ttu-id="32ad1-195">Hello időtartama listájából válassza ki a időtartama</span><span class="sxs-lookup"><span data-stu-id="32ad1-195">From hello duration list, select a duration</span></span>
   
    <span data-ttu-id="32ad1-196">b.</span><span class="sxs-lookup"><span data-stu-id="32ad1-196">b.</span></span> <span data-ttu-id="32ad1-197">Az alsó hello menüsávon kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="32ad1-197">On hello bottom bar, click **Save**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="32ad1-199">c.</span><span class="sxs-lookup"><span data-stu-id="32ad1-199">c.</span></span> <span data-ttu-id="32ad1-200">Másolja a hello kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="32ad1-200">Copy hello key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32ad1-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32ad1-201">Next Steps</span></span>
* <span data-ttu-id="32ad1-202">Reporting volna, például tooaccess hello hello Azure AD adatait API-val programozott módon?</span><span class="sxs-lookup"><span data-stu-id="32ad1-202">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="32ad1-203">Tekintse meg [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="32ad1-203">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="32ad1-204">Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="32ad1-204">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

