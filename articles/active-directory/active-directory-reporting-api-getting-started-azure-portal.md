---
title: "aaaGetting hello Azure AD reporting API használatába |} Microsoft Docs"
description: "Hogyan tooget használatába hello Azure Active Directory reporting API-val"
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
ms.openlocfilehash: bb7d72ba445daa367d7502889c38a605a16f26d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api"></a><span data-ttu-id="fd002-103">Ismerkedés az Azure Active Directory reporting API-val hello</span><span class="sxs-lookup"><span data-stu-id="fd002-103">Getting started with hello Azure Active Directory reporting API</span></span>

<span data-ttu-id="fd002-104">Az Azure Active Directory tartalmazza a különféle jelentések.</span><span class="sxs-lookup"><span data-stu-id="fd002-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="fd002-105">Ezek a jelentések adatainak hello lehet nagyon hasznos tooyour alkalmazások, például a SIEM rendszerek, naplózási és üzleti intelligencia eszközök.</span><span class="sxs-lookup"><span data-stu-id="fd002-105">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="fd002-106">hello Azure AD jelentéskészítési API-k olyan programozott hozzáférés toohello adatokat REST-alapú API-készlet.</span><span class="sxs-lookup"><span data-stu-id="fd002-106">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="fd002-107">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="fd002-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="fd002-108">Ez a cikk azt ismerteti hello hello Azure AD reporting használatába tooget szükséges API-k.</span><span class="sxs-lookup"><span data-stu-id="fd002-108">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="fd002-109">Hello a következő szakaszban található a további tudnivalók a hello naplózási és jelentkezzen be API-k.</span><span class="sxs-lookup"><span data-stu-id="fd002-109">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> 

<span data-ttu-id="fd002-110">Gyakran ismételt kérdések, olvassa el a [gyakran ismételt kérdések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="fd002-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="fd002-111">A problémák, kérjük, [fájlt egy támogatási jegy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="fd002-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="fd002-112">Tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="fd002-112">Learning map</span></span>
1. <span data-ttu-id="fd002-113">**Készítse elő** -tesztelheti az API-mintákat, jóvá kell toocomplete hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fd002-113">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="fd002-114">**Fedezze fel** -hello reporting API-k első benyomást kell szereznie:</span><span class="sxs-lookup"><span data-stu-id="fd002-114">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="fd002-115">Hello minták hello ellenőrzési API használatával</span><span class="sxs-lookup"><span data-stu-id="fd002-115">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="fd002-116">Hello minták használatával hello bejelentkezési tevékenység jelentés API-hoz.</span><span class="sxs-lookup"><span data-stu-id="fd002-116">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="fd002-117">**Testre szabhatja** -saját megoldás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="fd002-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="fd002-118">Hello naplózási API-hivatkozás használata</span><span class="sxs-lookup"><span data-stu-id="fd002-118">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="fd002-119">Hello bejelentkezési tevékenység jelentés segítségével API-referencia</span><span class="sxs-lookup"><span data-stu-id="fd002-119">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="fd002-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd002-120">Next Steps</span></span>
<span data-ttu-id="fd002-121">Ha fejezetét toosee összes elérhető Azure AD Graph API végpontok, használja ezt a hivatkozást: [https://graph.windows.net/tenant-name/activities/$ metaadatok? api-version = béta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="fd002-121">If you are curious toosee all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

