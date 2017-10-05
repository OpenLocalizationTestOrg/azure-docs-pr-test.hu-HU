---
title: "Bevezetés az Azure AD reporting API használatába |} Microsoft Docs"
description: "Ismerkedés az Azure Active Directory reporting API-val"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9944cbd2b1b7c4acb18d37da1394c0bbc170f77d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a><span data-ttu-id="edb0d-103">Bevezetés az Azure Active Directory reporting API használatába</span><span class="sxs-lookup"><span data-stu-id="edb0d-103">Getting started with the Azure Active Directory reporting API</span></span>

<span data-ttu-id="edb0d-104">Az Azure Active Directory tartalmazza a különféle jelentések.</span><span class="sxs-lookup"><span data-stu-id="edb0d-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="edb0d-105">Ezen jelentések adatai nagyon hasznosak lehetnek az alkalmazások számára, például a SIEM rendszerekhez, a naplózáshoz és az üzleti intelligencia eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="edb0d-105">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="edb0d-106">Az Azure AD-jelentéskészítés API-k REST-alapú API-kon keresztül biztosítják az adatok szoftveres hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="edb0d-106">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="edb0d-107">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="edb0d-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="edb0d-108">Ez a cikk azt ismerteti a kell Ismerkedés az Azure AD reporting API-k.</span><span class="sxs-lookup"><span data-stu-id="edb0d-108">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="edb0d-109">A következő szakaszban hogy a naplózás használatával kapcsolatos további részletekért és jelentkezzen be API-k.</span><span class="sxs-lookup"><span data-stu-id="edb0d-109">In the next section, you find more details about using the audit and sign-in APIs.</span></span> 

<span data-ttu-id="edb0d-110">Gyakran ismételt kérdések, olvassa el a [gyakran ismételt kérdések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="edb0d-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="edb0d-111">A problémák, kérjük, [fájlt egy támogatási jegy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="edb0d-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="edb0d-112">Tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="edb0d-112">Learning map</span></span>
1. <span data-ttu-id="edb0d-113">**Készítse elő** -előtt tesztelheti az API-mintákat, végre kell hajtania a [Előfeltételek az Azure AD reporting API eléréséhez](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="edb0d-113">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="edb0d-114">**Fedezze fel** -jelentéskészítési API-k első benyomást beolvasása:</span><span class="sxs-lookup"><span data-stu-id="edb0d-114">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="edb0d-115">A mintákat a ellenőrzési API használatával</span><span class="sxs-lookup"><span data-stu-id="edb0d-115">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="edb0d-116">A mintákat a bejelentkezési tevékenység jelentés API használatával</span><span class="sxs-lookup"><span data-stu-id="edb0d-116">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="edb0d-117">**Testre szabhatja** -saját megoldás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="edb0d-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="edb0d-118">A naplózási API-hivatkozás használata</span><span class="sxs-lookup"><span data-stu-id="edb0d-118">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="edb0d-119">A bejelentkezési tevékenység jelentéshivatkozás API használatával</span><span class="sxs-lookup"><span data-stu-id="edb0d-119">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="edb0d-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edb0d-120">Next Steps</span></span>
<span data-ttu-id="edb0d-121">Ha fejezetét megtekintéséhez az összes elérhető Azure AD Graph API-végpontok, akkor erre a hivatkozásra: [https://graph.windows.net/tenant-name/activities/$ metaadatok? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="edb0d-121">If you are curious to see all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

