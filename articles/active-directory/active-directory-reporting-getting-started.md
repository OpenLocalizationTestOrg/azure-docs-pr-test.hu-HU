---
title: "Azure Active Directory Reporting: első lépések | Microsoft Docs"
description: "Listák hello az Azure Active Directory reportingban elérhető különböző jelentéseket"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a><span data-ttu-id="06eb7-103">Bevezetés az Azure Active Directory Premium Reporting használatába</span><span class="sxs-lookup"><span data-stu-id="06eb7-103">Getting started with Azure Active Directory Reporting</span></span>
## <a name="what-it-is"></a><span data-ttu-id="06eb7-104">Mi ez?</span><span class="sxs-lookup"><span data-stu-id="06eb7-104">What it is</span></span>
<span data-ttu-id="06eb7-105">Az Azure Active Directory (Azure AD) biztonsági, naplózási és tevékenységjelentéseket biztosít a címtárához.</span><span class="sxs-lookup"><span data-stu-id="06eb7-105">Azure Active Directory (Azure AD) includes security, activity, and audit reports for your directory.</span></span> <span data-ttu-id="06eb7-106">Itt olvashat egy listát hello jelentéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="06eb7-106">Here's a list of hello reports included:</span></span>

### <a name="security-reports"></a><span data-ttu-id="06eb7-107">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="06eb7-107">Security reports</span></span>
* <span data-ttu-id="06eb7-108">Bejelentkezések ismeretlen forrásokról</span><span class="sxs-lookup"><span data-stu-id="06eb7-108">Sign-ins from unknown sources</span></span>
* <span data-ttu-id="06eb7-109">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="06eb7-109">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="06eb7-110">Bejelentkezések különböző földrajzi régiókból</span><span class="sxs-lookup"><span data-stu-id="06eb7-110">Sign-ins from multiple geographies</span></span>
* <span data-ttu-id="06eb7-111">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="06eb7-111">Sign-ins from IP addresses with suspicious activity</span></span>
* <span data-ttu-id="06eb7-112">Rendszertelen bejelentkezési tevékenység</span><span class="sxs-lookup"><span data-stu-id="06eb7-112">Irregular sign-in activity</span></span>
* <span data-ttu-id="06eb7-113">Bejelentkezések potenciálisan fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="06eb7-113">Sign-ins from possibly infected devices</span></span>
* <span data-ttu-id="06eb7-114">Rendellenes bejelentkezési tevékenységet mutató felhasználók</span><span class="sxs-lookup"><span data-stu-id="06eb7-114">Users with anomalous sign-in activity</span></span>

### <a name="activity-reports"></a><span data-ttu-id="06eb7-115">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="06eb7-115">Activity reports</span></span>
* <span data-ttu-id="06eb7-116">Alkalmazáshasználat: összegzés</span><span class="sxs-lookup"><span data-stu-id="06eb7-116">Application usage: summary</span></span>
* <span data-ttu-id="06eb7-117">Alkalmazáshasználat: részletes</span><span class="sxs-lookup"><span data-stu-id="06eb7-117">Application usage: detailed</span></span>
* <span data-ttu-id="06eb7-118">Alkalmazás irányítópultja</span><span class="sxs-lookup"><span data-stu-id="06eb7-118">Application dashboard</span></span>
* <span data-ttu-id="06eb7-119">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="06eb7-119">Account provisioning errors</span></span>
* <span data-ttu-id="06eb7-120">Egyéni felhasználói eszközök</span><span class="sxs-lookup"><span data-stu-id="06eb7-120">Individual user devices</span></span>
* <span data-ttu-id="06eb7-121">Egyéni felhasználói tevékenység</span><span class="sxs-lookup"><span data-stu-id="06eb7-121">Individual user Activity</span></span>
* <span data-ttu-id="06eb7-122">Csoporttevékenység-jelentés</span><span class="sxs-lookup"><span data-stu-id="06eb7-122">Groups activity report</span></span>
* <span data-ttu-id="06eb7-123">Jelszó-visszaállítási regisztrációs tevékenységjelentés</span><span class="sxs-lookup"><span data-stu-id="06eb7-123">Password Reset Registration Activity Report</span></span>
* <span data-ttu-id="06eb7-124">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="06eb7-124">Password reset activity</span></span>

### <a name="audit-reports"></a><span data-ttu-id="06eb7-125">Naplózási jelentések</span><span class="sxs-lookup"><span data-stu-id="06eb7-125">Audit reports</span></span>
* <span data-ttu-id="06eb7-126">Címtárnaplózási jelentés</span><span class="sxs-lookup"><span data-stu-id="06eb7-126">Directory audit report</span></span>

> [!TIP]
> <span data-ttu-id="06eb7-127">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="06eb7-127">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="how-it-works"></a><span data-ttu-id="06eb7-128">Működés</span><span class="sxs-lookup"><span data-stu-id="06eb7-128">How it works</span></span>
### <a name="reporting-pipeline"></a><span data-ttu-id="06eb7-129">Jelentéskészítési folyamat</span><span class="sxs-lookup"><span data-stu-id="06eb7-129">Reporting pipeline</span></span>
<span data-ttu-id="06eb7-130">hello jelentéskészítési folyamat három fő lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="06eb7-130">hello reporting pipeline consists of three main steps.</span></span> <span data-ttu-id="06eb7-131">Minden alkalommal, amikor egy felhasználó bejelentkezik, vagy hitelesítéskor, hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="06eb7-131">Every time a user signs in, or an authentication is made, hello following happens:</span></span>

* <span data-ttu-id="06eb7-132">Először hello felhasználó hitelesítése (sikeresen vagy sikertelenül), és hello eredmény hello Azure Active Directory szolgáltatás adatbázisaiban tárolja.</span><span class="sxs-lookup"><span data-stu-id="06eb7-132">First, hello user is authenticated (successfully or unsuccessfully), and hello result is stored in hello Azure Active Directory service databases.</span></span>
* <span data-ttu-id="06eb7-133">Rendszeres időközönként minden friss bejelentkezést feldolgoz.</span><span class="sxs-lookup"><span data-stu-id="06eb7-133">At regular intervals, all recent sign ins are processed.</span></span> <span data-ttu-id="06eb7-134">Ezen a ponton a rendellenes tevékenységeket észlelő, illetve biztonságos algoritmusok gyanús tevékenységeket keresnek az összes legutóbbi bejelentkezésben.</span><span class="sxs-lookup"><span data-stu-id="06eb7-134">At this point, our security and anomalous activity algorithms are searching all recent sign ins for suspicious activity.</span></span>
* <span data-ttu-id="06eb7-135">A feldolgozás után hello jelentések írása, a gyorsítótárba, és kiad a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="06eb7-135">After processing, hello reports are written, cached, and served in hello Azure classic portal.</span></span>

### <a name="report-generation-times"></a><span data-ttu-id="06eb7-136">Előállítási idők jelentése</span><span class="sxs-lookup"><span data-stu-id="06eb7-136">Report generation times</span></span>
<span data-ttu-id="06eb7-137">Lejáró toohello nagy mennyiségű hitelesítések és bejelentkezési hello által feldolgozott modulok az Azure AD platform hello legutóbbi bejelentkezések feldolgozása, átlagosan egy órával korábbiak.</span><span class="sxs-lookup"><span data-stu-id="06eb7-137">Due toohello large volume of authentications and sign ins processed by hello Azure AD platform, hello most recent sign-ins processed are, on average, one hour old.</span></span> <span data-ttu-id="06eb7-138">Ritka esetekben too8 óra tooprocess hello legutóbbi bejelentkezések azt is tarthat.</span><span class="sxs-lookup"><span data-stu-id="06eb7-138">In rare cases, it may take up too8 hours tooprocess hello most recent sign-ins.</span></span>

<span data-ttu-id="06eb7-139">Hello súgószöveg el az egyes jelentések tetején hello megvizsgálásával hello legutóbbi feldolgozott bejelentkezés találja.</span><span class="sxs-lookup"><span data-stu-id="06eb7-139">You can find hello most recent processed sign-in by examining hello help text at hello top of each report.</span></span>

![Az egyes jelentések hello tetején súgószöveg](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> <span data-ttu-id="06eb7-141">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="06eb7-141">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="getting-started"></a><span data-ttu-id="06eb7-142">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="06eb7-142">Getting started</span></span>
### <a name="sign-into-hello-azure-classic-portal"></a><span data-ttu-id="06eb7-143">Jelentkezzen be hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="06eb7-143">Sign into hello Azure classic portal</span></span>
<span data-ttu-id="06eb7-144">Először, szüksége lesz toosign történő hello [a klasszikus Azure portálon](https://manage.windowsazure.com) globális vagy megfelelőségi rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="06eb7-144">First, you'll need toosign into hello [Azure classic portal](https://manage.windowsazure.com)  as a global or compliance administrator.</span></span> <span data-ttu-id="06eb7-145">Egy Azure-előfizetés szolgáltatás-rendszergazda vagy társadminisztrátorának kell lennie, vagy használja hello "hozzáférési tooAzure AD" Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="06eb7-145">You must also be an Azure subscription service administrator or co-administrator, or be using hello "Access tooAzure AD" Azure subscription.</span></span>

### <a name="navigate-tooreports"></a><span data-ttu-id="06eb7-146">Keresse meg a tooReports</span><span class="sxs-lookup"><span data-stu-id="06eb7-146">Navigate tooReports</span></span>
<span data-ttu-id="06eb7-147">tooview jelentések, keresse meg a toohello jelentések lapot a címtár hello tetején.</span><span class="sxs-lookup"><span data-stu-id="06eb7-147">tooview Reports, navigate toohello Reports tab at hello top of your directory.</span></span>

<span data-ttu-id="06eb7-148">Ha az első alkalommal hello jelentések megtekintése, szüksége lesz tooagree tooa párbeszédpanel hello jelentések megtekintése előtt.</span><span class="sxs-lookup"><span data-stu-id="06eb7-148">If this is your first time viewing hello reports, you'll need tooagree tooa dialog box before you can view hello reports.</span></span> <span data-ttu-id="06eb7-149">Ez az, hogy a rendszer a szervezet tooview rendszergazdái elfogadható tooensure ezeket az adatokat, amelyek lehet, hogy egyes országokban bizalmas információnak számítanak.</span><span class="sxs-lookup"><span data-stu-id="06eb7-149">This is tooensure that it's acceptable for admins in your organization tooview this data, which may be considered private information in some countries.</span></span>

![Párbeszédpanel](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a><span data-ttu-id="06eb7-151">Az egyes jelentések megismerése</span><span class="sxs-lookup"><span data-stu-id="06eb7-151">Explore each report</span></span>
<span data-ttu-id="06eb7-152">Keresse meg minden jelentés toosee hello gyűjtött adatok típusától és hello bejelentkezések feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="06eb7-152">Navigate into each report toosee hello data being collected and hello sign-ins processed.</span></span> <span data-ttu-id="06eb7-153">Megtalálhatja a [összes hello jelentések itt listája](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="06eb7-153">You can find a [list of all hello reports here](active-directory-reporting-guide.md).</span></span>

![Minden jelentés](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a><span data-ttu-id="06eb7-155">Hello jelentések letöltése CSV-ként</span><span class="sxs-lookup"><span data-stu-id="06eb7-155">Download hello reports as CSV</span></span>
<span data-ttu-id="06eb7-156">Az egyes jelentések letölthetők CSV-fájlként (vesszővel tagolt adatfájlként).</span><span class="sxs-lookup"><span data-stu-id="06eb7-156">Each report can be downloaded as a CSV (comma-separated value) file.</span></span> <span data-ttu-id="06eb7-157">Ezeket a fájlokat, az Excel, a powerbi-ban vagy a programok toofurther elemezheti az adatokat külső analysis is használhatja.</span><span class="sxs-lookup"><span data-stu-id="06eb7-157">You can use these files in Excel, PowerBI or third-party analysis programs toofurther analyze your data.</span></span>

<span data-ttu-id="06eb7-158">a jelentés a fürt megosztott kötetei toodownload keresse meg a toohello jelentés, és kattintson a "Letöltés" hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="06eb7-158">toodownload any report as a CSV, navigate toohello report and click "Download" at hello bottom.</span></span>

![Letöltés gomb](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> <span data-ttu-id="06eb7-160">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="06eb7-160">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="06eb7-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06eb7-161">Next steps</span></span>
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a><span data-ttu-id="06eb7-162">Rendellenes bejelentkezési tevékenységek riasztásainak testreszabása</span><span class="sxs-lookup"><span data-stu-id="06eb7-162">Customize alerts for anomalous sign in activity</span></span>
<span data-ttu-id="06eb7-163">Keresse meg a könyvtár toohello "Beállítása" lapon.</span><span class="sxs-lookup"><span data-stu-id="06eb7-163">Navigate toohello "Configure" tab of your directory.</span></span>

<span data-ttu-id="06eb7-164">Görgessen toohello "Értesítések" szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="06eb7-164">Scroll toohello "Notifications" section.</span></span>

<span data-ttu-id="06eb7-165">Engedélyezi vagy letiltja a hello "E-mail-értesítések a rendellenes bejelentkezések" szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="06eb7-165">Enable or disable hello "Email Notifications of Anomalous sign-ins" section.</span></span>

![hello értesítések szakasz](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a><span data-ttu-id="06eb7-167">Azure AD jelentéskészítési API hello integrálása</span><span class="sxs-lookup"><span data-stu-id="06eb7-167">Integrate with hello Azure AD Reporting API</span></span>
<span data-ttu-id="06eb7-168">Lásd: [Ismerkedés a Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="06eb7-168">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

### <a name="engage-multi-factor-authentication-on-users"></a><span data-ttu-id="06eb7-169">Multi-Factor Authentication engedélyezése a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="06eb7-169">Engage Multi-Factor Authentication on users</span></span>
<span data-ttu-id="06eb7-170">Válasszon ki egy felhasználót egy jelentésben.</span><span class="sxs-lookup"><span data-stu-id="06eb7-170">Select a user in a report.</span></span>

<span data-ttu-id="06eb7-171">Gombra hello "MFA engedélyezése" hello hello képernyő aljára.</span><span class="sxs-lookup"><span data-stu-id="06eb7-171">Click hello "Enable MFA" button at hello bottom of hello screen.</span></span>

![hello multi-factor Authentication gomb üdvözlő képernyőt hello aljához](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> <span data-ttu-id="06eb7-173">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="06eb7-173">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="learn-more"></a><span data-ttu-id="06eb7-174">Részletek</span><span class="sxs-lookup"><span data-stu-id="06eb7-174">Learn more</span></span>
### <a name="audit-events"></a><span data-ttu-id="06eb7-175">Események naplózása</span><span class="sxs-lookup"><span data-stu-id="06eb7-175">Audit events</span></span>
<span data-ttu-id="06eb7-176">Ismerje meg, milyen eseményeket naplóz hello címtár [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="06eb7-176">Learn about what events are audited in hello directory in [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span></span>

### <a name="api-integration"></a><span data-ttu-id="06eb7-177">API-integráció</span><span class="sxs-lookup"><span data-stu-id="06eb7-177">API Integration</span></span>
<span data-ttu-id="06eb7-178">Lásd: [Ismerkedés a Reporting API hello](active-directory-reporting-api-getting-started.md) és hello [API-referenciadokumentáció](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span><span class="sxs-lookup"><span data-stu-id="06eb7-178">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md) and hello [API reference documentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span></span>

### <a name="get-in-touch"></a><span data-ttu-id="06eb7-179">Kapcsolatfelvétel</span><span class="sxs-lookup"><span data-stu-id="06eb7-179">Get in touch</span></span>
<span data-ttu-id="06eb7-180">Az [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) e-mail címre bármilyen észrevételét, problémáját vagy kérdését elküldheti.</span><span class="sxs-lookup"><span data-stu-id="06eb7-180">Email [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) for feedback, help, or any questions you might have.</span></span>

> [!TIP]
> <span data-ttu-id="06eb7-181">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="06eb7-181">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

