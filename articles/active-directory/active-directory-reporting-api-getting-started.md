---
title: "Az Azure AD reporting API a klasszikus Azure AD portálon az első lépések |} Microsoft Docs"
description: "Ismerkedés az Azure Active Directory reporting API-val"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5e98b660fe19bb8abebf1c3b996b6295a6c4e728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a><span data-ttu-id="f3a8a-103">Az Azure Active Directory reporting API-val a klasszikus Azure AD portálon az első lépések</span><span class="sxs-lookup"><span data-stu-id="f3a8a-103">Getting started with the Azure Active Directory reporting API on the Azure AD classic portal</span></span>
<span data-ttu-id="f3a8a-104">*Ez a témakör része a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="f3a8a-104">*This topic is part of the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="f3a8a-105">Az Azure Active Directory tartalmazza a különféle jelentések.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="f3a8a-106">Ezen jelentések adatai nagyon hasznosak lehetnek az alkalmazások számára, például a SIEM rendszerekhez, a naplózáshoz és az üzleti intelligencia eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-106">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="f3a8a-107">Az Azure AD-jelentéskészítés API-k REST-alapú API-kon keresztül biztosítják az adatok szoftveres hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-107">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="f3a8a-108">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="f3a8a-109">Ez a cikk azt ismerteti a kell Ismerkedés az Azure AD reporting API-k.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-109">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="f3a8a-110">A következő szakaszban hogy a naplózás használatával kapcsolatos további részletekért és jelentkezzen be API-k.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-110">In the next section, you find more details about using the audit and sign-in APIs.</span></span> <span data-ttu-id="f3a8a-111">Minden más API-k esetében, tekintse meg a [az Azure AD-jelentéseket és events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) cikk.</span><span class="sxs-lookup"><span data-stu-id="f3a8a-111">For all other APIs, see the [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="f3a8a-112">Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f3a8a-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="f3a8a-113">Tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="f3a8a-113">Learning map</span></span>
1. <span data-ttu-id="f3a8a-114">**Készítse elő** -előtt tesztelheti az API-mintákat, végre kell hajtania a [Előfeltételek az Azure AD reporting API eléréséhez](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="f3a8a-114">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="f3a8a-115">**Fedezze fel** -jelentéskészítési API-k első benyomást beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f3a8a-115">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="f3a8a-116">A mintákat a ellenőrzési API használatával</span><span class="sxs-lookup"><span data-stu-id="f3a8a-116">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="f3a8a-117">A mintákat a bejelentkezési tevékenység jelentés API használatával</span><span class="sxs-lookup"><span data-stu-id="f3a8a-117">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="f3a8a-118">**Testre szabhatja** -saját megoldás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="f3a8a-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="f3a8a-119">A naplózási API-hivatkozás használata</span><span class="sxs-lookup"><span data-stu-id="f3a8a-119">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="f3a8a-120">A bejelentkezési tevékenység jelentéshivatkozás API használatával</span><span class="sxs-lookup"><span data-stu-id="f3a8a-120">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="f3a8a-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3a8a-121">Next Steps</span></span>
<span data-ttu-id="f3a8a-122">Ha fejezetét láthatja az összes elérhető Azure AD Graph API végpontjainak útvonalon [https://graph.windows.net/tenant-name/reports/$ metaadatok? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="f3a8a-122">If you are curious to see all of the available Azure AD Graph API endpoints by navigating to [https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

