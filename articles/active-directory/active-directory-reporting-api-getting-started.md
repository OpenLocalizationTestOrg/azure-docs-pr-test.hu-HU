---
title: "aaaGetting hello Azure AD jelentéskészítési API hello Azure AD-klasszikus portál használatába |} Microsoft Docs"
description: "Hogyan tooget használatába hello Azure Active Directory reporting API-val"
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
ms.openlocfilehash: 52e22d442650731fc6ed28991fc65f9182af0540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api-on-hello-azure-ad-classic-portal"></a><span data-ttu-id="7e047-103">Ismerkedés az Azure Active Directory reporting API-val az Azure AD hello klasszikus portál hello</span><span class="sxs-lookup"><span data-stu-id="7e047-103">Getting started with hello Azure Active Directory reporting API on hello Azure AD classic portal</span></span>
<span data-ttu-id="7e047-104">*Ez a témakör hello része [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="7e047-104">*This topic is part of hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="7e047-105">Az Azure Active Directory tartalmazza a különféle jelentések.</span><span class="sxs-lookup"><span data-stu-id="7e047-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="7e047-106">Ezek a jelentések adatainak hello lehet nagyon hasznos tooyour alkalmazások, például a SIEM rendszerek, naplózási és üzleti intelligencia eszközök.</span><span class="sxs-lookup"><span data-stu-id="7e047-106">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="7e047-107">hello Azure AD jelentéskészítési API-k olyan programozott hozzáférés toohello adatokat REST-alapú API-készlet.</span><span class="sxs-lookup"><span data-stu-id="7e047-107">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="7e047-108">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="7e047-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="7e047-109">Ez a cikk azt ismerteti hello hello Azure AD reporting használatába tooget szükséges API-k.</span><span class="sxs-lookup"><span data-stu-id="7e047-109">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="7e047-110">Hello a következő szakaszban található a további tudnivalók a hello naplózási és jelentkezzen be API-k.</span><span class="sxs-lookup"><span data-stu-id="7e047-110">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> <span data-ttu-id="7e047-111">Minden más API-k esetében, tekintse meg a hello [az Azure AD-jelentéseket és events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) cikk.</span><span class="sxs-lookup"><span data-stu-id="7e047-111">For all other APIs, see hello [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="7e047-112">Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7e047-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="7e047-113">Tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="7e047-113">Learning map</span></span>
1. <span data-ttu-id="7e047-114">**Készítse elő** -tesztelheti az API-mintákat, jóvá kell toocomplete hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="7e047-114">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="7e047-115">**Fedezze fel** -hello reporting API-k első benyomást kell szereznie:</span><span class="sxs-lookup"><span data-stu-id="7e047-115">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="7e047-116">Hello minták hello ellenőrzési API használatával</span><span class="sxs-lookup"><span data-stu-id="7e047-116">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="7e047-117">Hello minták használatával hello bejelentkezési tevékenység jelentés API-hoz.</span><span class="sxs-lookup"><span data-stu-id="7e047-117">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="7e047-118">**Testre szabhatja** -saját megoldás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="7e047-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="7e047-119">Hello naplózási API-hivatkozás használata</span><span class="sxs-lookup"><span data-stu-id="7e047-119">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="7e047-120">Hello bejelentkezési tevékenység jelentés segítségével API-referencia</span><span class="sxs-lookup"><span data-stu-id="7e047-120">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="7e047-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e047-121">Next Steps</span></span>
<span data-ttu-id="7e047-122">Ha kíváncsi toosee összes hello túl útvonalon elérhető Azure AD Graph API-végpontok[https://graph.windows.net/tenant-name/reports/$ metaadatok? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="7e047-122">If you are curious toosee all of hello available Azure AD Graph API endpoints by navigating too[https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

