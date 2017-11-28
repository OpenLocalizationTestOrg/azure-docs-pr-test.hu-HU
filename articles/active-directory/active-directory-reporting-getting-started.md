---
title: "Azure Active Directory Reporting: első lépések | Microsoft Docs"
description: "Felsorolja az Azure Active Directory Reportingban elérhető különböző jelentéseket."
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
ms.openlocfilehash: 5cd1ae6196d9cd63f97dc9d302442280ece23e40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a><span data-ttu-id="38c38-103">Bevezetés az Azure Active Directory Premium Reporting használatába</span><span class="sxs-lookup"><span data-stu-id="38c38-103">Getting started with Azure Active Directory Reporting</span></span>
## <a name="what-it-is"></a><span data-ttu-id="38c38-104">Mi ez?</span><span class="sxs-lookup"><span data-stu-id="38c38-104">What it is</span></span>
<span data-ttu-id="38c38-105">Az Azure Active Directory (Azure AD) biztonsági, naplózási és tevékenységjelentéseket biztosít a címtárához.</span><span class="sxs-lookup"><span data-stu-id="38c38-105">Azure Active Directory (Azure AD) includes security, activity, and audit reports for your directory.</span></span> <span data-ttu-id="38c38-106">A benne foglalt jelentések listája:</span><span class="sxs-lookup"><span data-stu-id="38c38-106">Here's a list of the reports included:</span></span>

### <a name="security-reports"></a><span data-ttu-id="38c38-107">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="38c38-107">Security reports</span></span>
* <span data-ttu-id="38c38-108">Bejelentkezések ismeretlen forrásokról</span><span class="sxs-lookup"><span data-stu-id="38c38-108">Sign-ins from unknown sources</span></span>
* <span data-ttu-id="38c38-109">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="38c38-109">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="38c38-110">Bejelentkezések különböző földrajzi régiókból</span><span class="sxs-lookup"><span data-stu-id="38c38-110">Sign-ins from multiple geographies</span></span>
* <span data-ttu-id="38c38-111">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="38c38-111">Sign-ins from IP addresses with suspicious activity</span></span>
* <span data-ttu-id="38c38-112">Rendszertelen bejelentkezési tevékenység</span><span class="sxs-lookup"><span data-stu-id="38c38-112">Irregular sign-in activity</span></span>
* <span data-ttu-id="38c38-113">Bejelentkezések potenciálisan fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="38c38-113">Sign-ins from possibly infected devices</span></span>
* <span data-ttu-id="38c38-114">Rendellenes bejelentkezési tevékenységet mutató felhasználók</span><span class="sxs-lookup"><span data-stu-id="38c38-114">Users with anomalous sign-in activity</span></span>

### <a name="activity-reports"></a><span data-ttu-id="38c38-115">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="38c38-115">Activity reports</span></span>
* <span data-ttu-id="38c38-116">Alkalmazáshasználat: összegzés</span><span class="sxs-lookup"><span data-stu-id="38c38-116">Application usage: summary</span></span>
* <span data-ttu-id="38c38-117">Alkalmazáshasználat: részletes</span><span class="sxs-lookup"><span data-stu-id="38c38-117">Application usage: detailed</span></span>
* <span data-ttu-id="38c38-118">Alkalmazás irányítópultja</span><span class="sxs-lookup"><span data-stu-id="38c38-118">Application dashboard</span></span>
* <span data-ttu-id="38c38-119">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="38c38-119">Account provisioning errors</span></span>
* <span data-ttu-id="38c38-120">Egyéni felhasználói eszközök</span><span class="sxs-lookup"><span data-stu-id="38c38-120">Individual user devices</span></span>
* <span data-ttu-id="38c38-121">Egyéni felhasználói tevékenység</span><span class="sxs-lookup"><span data-stu-id="38c38-121">Individual user Activity</span></span>
* <span data-ttu-id="38c38-122">Csoporttevékenység-jelentés</span><span class="sxs-lookup"><span data-stu-id="38c38-122">Groups activity report</span></span>
* <span data-ttu-id="38c38-123">Jelszó-visszaállítási regisztrációs tevékenységjelentés</span><span class="sxs-lookup"><span data-stu-id="38c38-123">Password Reset Registration Activity Report</span></span>
* <span data-ttu-id="38c38-124">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="38c38-124">Password reset activity</span></span>

### <a name="audit-reports"></a><span data-ttu-id="38c38-125">Naplózási jelentések</span><span class="sxs-lookup"><span data-stu-id="38c38-125">Audit reports</span></span>
* <span data-ttu-id="38c38-126">Címtárnaplózási jelentés</span><span class="sxs-lookup"><span data-stu-id="38c38-126">Directory audit report</span></span>

> [!TIP]
> <span data-ttu-id="38c38-127">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="38c38-127">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="how-it-works"></a><span data-ttu-id="38c38-128">Működés</span><span class="sxs-lookup"><span data-stu-id="38c38-128">How it works</span></span>
### <a name="reporting-pipeline"></a><span data-ttu-id="38c38-129">Jelentéskészítési folyamat</span><span class="sxs-lookup"><span data-stu-id="38c38-129">Reporting pipeline</span></span>
<span data-ttu-id="38c38-130">A jelentéskészítési folyamat három fő lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="38c38-130">The reporting pipeline consists of three main steps.</span></span> <span data-ttu-id="38c38-131">Minden felhasználói bejelentkezéskor vagy hitelesítéskor a következő történik:</span><span class="sxs-lookup"><span data-stu-id="38c38-131">Every time a user signs in, or an authentication is made, the following happens:</span></span>

* <span data-ttu-id="38c38-132">Először a rendszer hitelesíti a felhasználót (sikeresen vagy sikertelenül), és az eredményt az Azure Active Directory szolgáltatás adatbázisaiban tárolja.</span><span class="sxs-lookup"><span data-stu-id="38c38-132">First, the user is authenticated (successfully or unsuccessfully), and the result is stored in the Azure Active Directory service databases.</span></span>
* <span data-ttu-id="38c38-133">Rendszeres időközönként minden friss bejelentkezést feldolgoz.</span><span class="sxs-lookup"><span data-stu-id="38c38-133">At regular intervals, all recent sign ins are processed.</span></span> <span data-ttu-id="38c38-134">Ezen a ponton a rendellenes tevékenységeket észlelő, illetve biztonságos algoritmusok gyanús tevékenységeket keresnek az összes legutóbbi bejelentkezésben.</span><span class="sxs-lookup"><span data-stu-id="38c38-134">At this point, our security and anomalous activity algorithms are searching all recent sign ins for suspicious activity.</span></span>
* <span data-ttu-id="38c38-135">A feldolgozás után a rendszer minden jelentést leír, gyorsítótáraz és kiad a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="38c38-135">After processing, the reports are written, cached, and served in the Azure classic portal.</span></span>

### <a name="report-generation-times"></a><span data-ttu-id="38c38-136">Előállítási idők jelentése</span><span class="sxs-lookup"><span data-stu-id="38c38-136">Report generation times</span></span>
<span data-ttu-id="38c38-137">Az Azure AD platform által feldolgozott hitelesítések és bejelentkezések nagy száma miatt a legutóbb feldolgozott bejelentkezések átlagosan egy órával korábbiak.</span><span class="sxs-lookup"><span data-stu-id="38c38-137">Due to the large volume of authentications and sign ins processed by the Azure AD platform, the most recent sign-ins processed are, on average, one hour old.</span></span> <span data-ttu-id="38c38-138">Ritka esetben akár 8 órát is igénybe vehet a legutóbbi bejelentkezések feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="38c38-138">In rare cases, it may take up to 8 hours to process the most recent sign-ins.</span></span>

<span data-ttu-id="38c38-139">A legutóbb feldolgozott bejelentkezést az egyes jelentések tetején megjelenő súgószövegben találja.</span><span class="sxs-lookup"><span data-stu-id="38c38-139">You can find the most recent processed sign-in by examining the help text at the top of each report.</span></span>

![Az egyes jelentések tetején megjelenő súgószöveg](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> <span data-ttu-id="38c38-141">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="38c38-141">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="getting-started"></a><span data-ttu-id="38c38-142">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="38c38-142">Getting started</span></span>
### <a name="sign-into-the-azure-classic-portal"></a><span data-ttu-id="38c38-143">Bejelentkezés a klasszikus Azure portálra</span><span class="sxs-lookup"><span data-stu-id="38c38-143">Sign into the Azure classic portal</span></span>
<span data-ttu-id="38c38-144">Először globális rendszergazdaként vagy szabályozási ügyintézőként be kell jelentkeznie a [klasszikus Azure portálra](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="38c38-144">First, you'll need to sign into the [Azure classic portal](https://manage.windowsazure.com)  as a global or compliance administrator.</span></span> <span data-ttu-id="38c38-145">Emellett Azure-előfizetési szolgáltatási rendszergazdának vagy társadminisztrátornak is kell lennie, vagy a „Hozzáférés az Azure AD-hez” Azure-előfizetést kell használnia.</span><span class="sxs-lookup"><span data-stu-id="38c38-145">You must also be an Azure subscription service administrator or co-administrator, or be using the "Access to Azure AD" Azure subscription.</span></span>

### <a name="navigate-to-reports"></a><span data-ttu-id="38c38-146">Navigálás a jelentésekhez</span><span class="sxs-lookup"><span data-stu-id="38c38-146">Navigate to Reports</span></span>
<span data-ttu-id="38c38-147">A jelentések megtekintéséhez nyissa meg a Jelentések lapot a címtár tetején.</span><span class="sxs-lookup"><span data-stu-id="38c38-147">To view Reports, navigate to the Reports tab at the top of your directory.</span></span>

<span data-ttu-id="38c38-148">Ha most nyitja meg először a jelentéseket, akkor a megtekintésük előtt el kell fogadnia a megjelenő párbeszédpanel feltételeit.</span><span class="sxs-lookup"><span data-stu-id="38c38-148">If this is your first time viewing the reports, you'll need to agree to a dialog box before you can view the reports.</span></span> <span data-ttu-id="38c38-149">A rendszer így győződik meg arról, hogy elfogadható, hogy a szervezet rendszergazdái megtekintik ezeket az adatokat, amelyek egyes országokban bizalmas információnak számítanak.</span><span class="sxs-lookup"><span data-stu-id="38c38-149">This is to ensure that it's acceptable for admins in your organization to view this data, which may be considered private information in some countries.</span></span>

![Párbeszédpanel](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a><span data-ttu-id="38c38-151">Az egyes jelentések megismerése</span><span class="sxs-lookup"><span data-stu-id="38c38-151">Explore each report</span></span>
<span data-ttu-id="38c38-152">Lépjen egyenként a jelentésekre, így megtekintheti az összegyűjtött adatokat és a feldolgozott bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="38c38-152">Navigate into each report to see the data being collected and the sign-ins processed.</span></span> <span data-ttu-id="38c38-153">Itt találja meg [az összes jelentés listáját](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="38c38-153">You can find a [list of all the reports here](active-directory-reporting-guide.md).</span></span>

![Minden jelentés](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a><span data-ttu-id="38c38-155">A jelentések letöltése CSV-fájlként</span><span class="sxs-lookup"><span data-stu-id="38c38-155">Download the reports as CSV</span></span>
<span data-ttu-id="38c38-156">Az egyes jelentések letölthetők CSV-fájlként (vesszővel tagolt adatfájlként).</span><span class="sxs-lookup"><span data-stu-id="38c38-156">Each report can be downloaded as a CSV (comma-separated value) file.</span></span> <span data-ttu-id="38c38-157">Ezeket a fájlokat felhasználhatja az Excelben, a PowerBI-ban vagy más külső elemzőprogramokban az adatok további elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="38c38-157">You can use these files in Excel, PowerBI or third-party analysis programs to further analyze your data.</span></span>

<span data-ttu-id="38c38-158">Ha egy jelentést CSV-formátumban szeretne letölteni, navigáljon a jelentéshez, és kattintson lent a „Letöltés” gombra.</span><span class="sxs-lookup"><span data-stu-id="38c38-158">To download any report as a CSV, navigate to the report and click "Download" at the bottom.</span></span>

![Letöltés gomb](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> <span data-ttu-id="38c38-160">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="38c38-160">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="38c38-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38c38-161">Next steps</span></span>
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a><span data-ttu-id="38c38-162">Rendellenes bejelentkezési tevékenységek riasztásainak testreszabása</span><span class="sxs-lookup"><span data-stu-id="38c38-162">Customize alerts for anomalous sign in activity</span></span>
<span data-ttu-id="38c38-163">Navigáljon a „Konfigurálás” lapra a címtárban.</span><span class="sxs-lookup"><span data-stu-id="38c38-163">Navigate to the "Configure" tab of your directory.</span></span>

<span data-ttu-id="38c38-164">Görgessen az „Értesítések” szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="38c38-164">Scroll to the "Notifications" section.</span></span>

<span data-ttu-id="38c38-165">Engedélyezze vagy tiltsa le az „Email Notifications of Anomalous sign-ins” (Értesítés e-mailben a rendellenes bejelentkezésekről) szakaszt.</span><span class="sxs-lookup"><span data-stu-id="38c38-165">Enable or disable the "Email Notifications of Anomalous sign-ins" section.</span></span>

![Az Értesítések szakasz](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a><span data-ttu-id="38c38-167">Integráció az Azure AD Reporting API-val</span><span class="sxs-lookup"><span data-stu-id="38c38-167">Integrate with the Azure AD Reporting API</span></span>
<span data-ttu-id="38c38-168">Lásd: [Bevezetés a Reporting API használatába](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="38c38-168">See [Getting started with the Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

### <a name="engage-multi-factor-authentication-on-users"></a><span data-ttu-id="38c38-169">Multi-Factor Authentication engedélyezése a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="38c38-169">Engage Multi-Factor Authentication on users</span></span>
<span data-ttu-id="38c38-170">Válasszon ki egy felhasználót egy jelentésben.</span><span class="sxs-lookup"><span data-stu-id="38c38-170">Select a user in a report.</span></span>

<span data-ttu-id="38c38-171">Kattintson a képernyő alján található „MFA engedélyezése” gombra.</span><span class="sxs-lookup"><span data-stu-id="38c38-171">Click the "Enable MFA" button at the bottom of the screen.</span></span>

![A Multi-Factor Authentication gomb a képernyő alján](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> <span data-ttu-id="38c38-173">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="38c38-173">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="learn-more"></a><span data-ttu-id="38c38-174">Részletek</span><span class="sxs-lookup"><span data-stu-id="38c38-174">Learn more</span></span>
### <a name="audit-events"></a><span data-ttu-id="38c38-175">Események naplózása</span><span class="sxs-lookup"><span data-stu-id="38c38-175">Audit events</span></span>
<span data-ttu-id="38c38-176">Megtudhatja, milyen eseményeket naplóz az [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md) a címtárban.</span><span class="sxs-lookup"><span data-stu-id="38c38-176">Learn about what events are audited in the directory in [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span></span>

### <a name="api-integration"></a><span data-ttu-id="38c38-177">API-integráció</span><span class="sxs-lookup"><span data-stu-id="38c38-177">API Integration</span></span>
<span data-ttu-id="38c38-178">Lásd: [Bevezetés a Reporting API használatába](active-directory-reporting-api-getting-started.md) és [API-referenciadokumentáció](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span><span class="sxs-lookup"><span data-stu-id="38c38-178">See [Getting started with the Reporting API](active-directory-reporting-api-getting-started.md) and the [API reference documentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span></span>

### <a name="get-in-touch"></a><span data-ttu-id="38c38-179">Kapcsolatfelvétel</span><span class="sxs-lookup"><span data-stu-id="38c38-179">Get in touch</span></span>
<span data-ttu-id="38c38-180">Az [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) e-mail címre bármilyen észrevételét, problémáját vagy kérdését elküldheti.</span><span class="sxs-lookup"><span data-stu-id="38c38-180">Email [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) for feedback, help, or any questions you might have.</span></span>

> [!TIP]
> <span data-ttu-id="38c38-181">Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).</span><span class="sxs-lookup"><span data-stu-id="38c38-181">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

