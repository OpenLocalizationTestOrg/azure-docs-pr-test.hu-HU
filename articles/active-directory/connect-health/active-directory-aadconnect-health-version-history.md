---
title: "Azure AD Connect Health Version History (Az Azure AD Connect Health verzióelőzményei)"
description: "Ez a dokumentum ismerteti a kiadások, az Azure AD Connect Health és mi ezeket a kiadásokat szerepel."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6c990a184d44771c78330f54f518bb4c35a36a35
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-health-version-release-history"></a><span data-ttu-id="151af-103">Azure AD Connect Health: Version History (Az Azure AD Connect Health verzióelőzményei)</span><span class="sxs-lookup"><span data-stu-id="151af-103">Azure AD Connect Health: Version Release History</span></span>
<span data-ttu-id="151af-104">Az Azure Active Directory ügyfélszolgálata rendszeresen frissíti az Azure AD Connect Health új szolgáltatásait és funkcióit.</span><span class="sxs-lookup"><span data-stu-id="151af-104">The Azure Active Directory team regularly updates Azure AD Connect Health with new features and functionality.</span></span> <span data-ttu-id="151af-105">Ez a cikk felsorolja a kiadott szolgáltatások és verziókat.</span><span class="sxs-lookup"><span data-stu-id="151af-105">This article lists the versions and features that have been released.</span></span>

## <a name="october-2016"></a><span data-ttu-id="151af-106">2016. október</span><span class="sxs-lookup"><span data-stu-id="151af-106">October 2016</span></span>
<span data-ttu-id="151af-107">**Ügynök frissítése:**</span><span class="sxs-lookup"><span data-stu-id="151af-107">**Agent Update:**</span></span>

* <span data-ttu-id="151af-108">Az Azure AD Connect Health-ügynök az AD FS \(2.6.408.0 verziója\)</span><span class="sxs-lookup"><span data-stu-id="151af-108">Azure AD Connect Health agent for AD FS \(version 2.6.408.0\)</span></span>
  1. <span data-ttu-id="151af-109">A hitelesítési kérések ügyfél IP-címek kimutatására fejlesztései</span><span class="sxs-lookup"><span data-stu-id="151af-109">Improvements in detecting client IP addresses in authentication requests</span></span>
  2. <span data-ttu-id="151af-110">Hibajavítások kapcsolatos riasztások</span><span class="sxs-lookup"><span data-stu-id="151af-110">Bug Fixes related to Alerts</span></span>
* <span data-ttu-id="151af-111">Az Azure AD Connect Health-ügynök az AD DS (2.6.408.0 verzió)</span><span class="sxs-lookup"><span data-stu-id="151af-111">Azure AD Connect Health agent for AD DS (version 2.6.408.0)</span></span>
  1. <span data-ttu-id="151af-112">Riasztások kapcsolatos hibajavítások.</span><span class="sxs-lookup"><span data-stu-id="151af-112">Bug fixes related to Alerts.</span></span>
* <span data-ttu-id="151af-113">Az Azure AD Connect Health-ügynök (verzió: 2.6.353.0)-szinkronizáláshoz az Azure AD Connect 1.1.281.0 verziója, amely a</span><span class="sxs-lookup"><span data-stu-id="151af-113">Azure AD Connect Health agent for Sync (version 2.6.353.0) released with Azure AD Connect version 1.1.281.0</span></span>
  1. <span data-ttu-id="151af-114">A szinkronizálás hibajelentések adja meg a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="151af-114">Provide the required data for the Synchronization Error Reports</span></span>
  2. <span data-ttu-id="151af-115">Riasztások kapcsolatos hibajavítások</span><span class="sxs-lookup"><span data-stu-id="151af-115">Bug fixes related to Alerts</span></span>

<span data-ttu-id="151af-116">**Új előzetes verziójú funkciók:**</span><span class="sxs-lookup"><span data-stu-id="151af-116">**New preview features:**</span></span>

* <span data-ttu-id="151af-117">Szinkronizálási hiba jelentések az Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="151af-117">Synchronization Error Reports for Azure AD Connect</span></span>

<span data-ttu-id="151af-118">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="151af-118">**New features:**</span></span>

* <span data-ttu-id="151af-119">Az Azure AD Connect Health AD FS - IP-cím mező áll rendelkezésre a rossz felhasználónév/jelszó próbálkozó 50 felhasználó jelentést.</span><span class="sxs-lookup"><span data-stu-id="151af-119">Azure AD Connect Health for AD FS - IP address field is available in the report about top 50 users with bad username/password.</span></span>

## <a name="july-2016"></a><span data-ttu-id="151af-120">2016. július</span><span class="sxs-lookup"><span data-stu-id="151af-120">July 2016</span></span>
<span data-ttu-id="151af-121">**Új előzetes verziójú funkciók:**</span><span class="sxs-lookup"><span data-stu-id="151af-121">**New preview features:**</span></span>

* <span data-ttu-id="151af-122">[Az Azure AD Connect Health AD DS](active-directory-aadconnect-health-adds.md).</span><span class="sxs-lookup"><span data-stu-id="151af-122">[Azure AD Connect Health for AD DS](active-directory-aadconnect-health-adds.md).</span></span>

## <a name="january-2016"></a><span data-ttu-id="151af-123">2016. január</span><span class="sxs-lookup"><span data-stu-id="151af-123">January 2016</span></span>
<span data-ttu-id="151af-124">**Ügynök frissítése:**</span><span class="sxs-lookup"><span data-stu-id="151af-124">**Agent Update:**</span></span>

* <span data-ttu-id="151af-125">Az Azure AD Connect Health-ügynök az AD FS (2.6.91.1512 verzió)</span><span class="sxs-lookup"><span data-stu-id="151af-125">Azure AD Connect Health agent for AD FS (version 2.6.91.1512)</span></span>

<span data-ttu-id="151af-126">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="151af-126">**New features:**</span></span>

* [<span data-ttu-id="151af-127">Teszt kapcsolódási eszköz az Azure AD Connect Health-ügynökök</span><span class="sxs-lookup"><span data-stu-id="151af-127">Test Connectivity Tool for Azure AD Connect Health Agents</span></span>](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a><span data-ttu-id="151af-128">2015. november</span><span class="sxs-lookup"><span data-stu-id="151af-128">November 2015</span></span>
<span data-ttu-id="151af-129">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="151af-129">**New features:**</span></span>

* <span data-ttu-id="151af-130">Támogatja az [szerepköralapú hozzáférés-vezérlés](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)</span><span class="sxs-lookup"><span data-stu-id="151af-130">Support for [Role Based Access Control](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)</span></span>

<span data-ttu-id="151af-131">**Új előzetes verziójú funkciók:**</span><span class="sxs-lookup"><span data-stu-id="151af-131">**New preview features:**</span></span>

* <span data-ttu-id="151af-132">[Az Azure AD Connect Health szinkronizálási szolgáltatás](active-directory-aadconnect-health-sync.md).</span><span class="sxs-lookup"><span data-stu-id="151af-132">[Azure AD Connect Health for sync](active-directory-aadconnect-health-sync.md).</span></span>

<span data-ttu-id="151af-133">**Javított problémák:**</span><span class="sxs-lookup"><span data-stu-id="151af-133">**Fixed issues:**</span></span>

* <span data-ttu-id="151af-134">Hibaelhárítás a ügynök regisztráció során tapasztalt hibákat.</span><span class="sxs-lookup"><span data-stu-id="151af-134">Bug fixes for errors seen during agent registrations.</span></span>

## <a name="september-2015"></a><span data-ttu-id="151af-135">2015. szeptember</span><span class="sxs-lookup"><span data-stu-id="151af-135">September 2015</span></span>
<span data-ttu-id="151af-136">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="151af-136">**New features:**</span></span>

* <span data-ttu-id="151af-137">Helytelen a felhasználónév-jelszó jelentés az AD FS Szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="151af-137">Wrong Username password report for AD FS</span></span>
* <span data-ttu-id="151af-138">Támogatja a nem hitelesített HTTP-Proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="151af-138">Support to configure Unauthenticated HTTP Proxy</span></span>
* <span data-ttu-id="151af-139">Ügynök konfigurálása Server core-on a támogatási szolgálathoz</span><span class="sxs-lookup"><span data-stu-id="151af-139">Support to configure agent on Server core</span></span>
* <span data-ttu-id="151af-140">Az AD FS riasztások fejlesztései</span><span class="sxs-lookup"><span data-stu-id="151af-140">Improvements to Alerts for AD FS</span></span>
* <span data-ttu-id="151af-141">Töltse fel az AD FS Szolgáltatásokhoz a kapcsolat és az Azure AD Connect Health-ügynök javítását.</span><span class="sxs-lookup"><span data-stu-id="151af-141">Improvements in Azure AD Connect Health Agent for AD FS for connectivity and data upload.</span></span>

<span data-ttu-id="151af-142">**Javított problémák:**</span><span class="sxs-lookup"><span data-stu-id="151af-142">**Fixed issues:**</span></span>

* <span data-ttu-id="151af-143">Hibaelhárítás a használati adatok feltárása az AD FS típusok.</span><span class="sxs-lookup"><span data-stu-id="151af-143">Bug fixes in Usage Insights for AD FS Error types.</span></span>

## <a name="june-2015"></a><span data-ttu-id="151af-144">2015. június</span><span class="sxs-lookup"><span data-stu-id="151af-144">June 2015</span></span>
<span data-ttu-id="151af-145">**Eredeti kiadását az Azure AD Connect Health AD FS és az AD FS Proxy.**</span><span class="sxs-lookup"><span data-stu-id="151af-145">**Initial release of Azure AD Connect Health for AD FS and AD FS Proxy.**</span></span>

<span data-ttu-id="151af-146">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="151af-146">**New features:**</span></span>

* <span data-ttu-id="151af-147">Riasztások értesítő e-mailek az AD FS és az AD FS-Proxy kiszolgálók figyelésére.</span><span class="sxs-lookup"><span data-stu-id="151af-147">Alerts for monitoring AD FS and AD FS Proxy servers with email notifications.</span></span>
* <span data-ttu-id="151af-148">Az AD FS topológia és AD FS teljesítményszámlálók minták könnyen elérhetők.</span><span class="sxs-lookup"><span data-stu-id="151af-148">Easy access to AD FS topology and patterns in AD FS Performance Counters.</span></span>
* <span data-ttu-id="151af-149">Trend a sikeres jogkivonat-kérelmeket az alkalmazásokat, a hitelesítési módszereket, a kérelmek hálózati helye stb szerint csoportosítva AD FS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="151af-149">Trend in successful token requests on AD FS servers grouped by Applications, Authentication Methods, Request Network Location etc.</span></span>
* <span data-ttu-id="151af-150">Alkalmazások, hiba típusok stb szerint csoportosítva AD FS-kiszolgáló a sikertelen kérelmek trendeket.</span><span class="sxs-lookup"><span data-stu-id="151af-150">Trends in failed request on AD FS servers grouped by Applications, Error Types etc.</span></span>
* <span data-ttu-id="151af-151">Egyszerűbb ügynök központi telepítése az Azure AD globális rendszergazda hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="151af-151">Simpler Agent Deployment using Azure AD Global Admin credentials.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="151af-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="151af-152">Next steps</span></span>
<span data-ttu-id="151af-153">További információ [a helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások megfigyelése a felhőben](active-directory-aadconnect-health.md).</span><span class="sxs-lookup"><span data-stu-id="151af-153">Learn more about [Monitor your on-premises identity infrastructure and synchronization services in the cloud](active-directory-aadconnect-health.md).</span></span>

