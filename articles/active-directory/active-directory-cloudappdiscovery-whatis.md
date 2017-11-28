---
title: "aaaFinding nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery |} Microsoft Docs"
description: "Információk keresése és alkalmazások kezelése az a Cloud App Discovery, melyek hello előnyei és annak működéséről."
services: active-directory
keywords: "a cloud app discovery alkalmazások kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a><span data-ttu-id="723c0-104">A Cloud App Discovery megállapítás nem felügyelt felhőalkalmazások</span><span class="sxs-lookup"><span data-stu-id="723c0-104">Finding unmanaged cloud applications with Cloud App Discovery</span></span>
## <a name="overview"></a><span data-ttu-id="723c0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="723c0-105">Overview</span></span>
<span data-ttu-id="723c0-106">A modern vállalatok informatikai részlegek ismerik gyakran nem minden hello felhőalapú alkalmazásokhoz, hogy a szervezet tagjai használjon toodo munkavégzést.</span><span class="sxs-lookup"><span data-stu-id="723c0-106">In modern enterprises, IT departments are often not aware of all hello cloud applications that members of their organization use toodo their work.</span></span> <span data-ttu-id="723c0-107">Miért a rendszergazdák jogosulatlan hozzáférés toocorporate adatokat, a lehetséges adatszivárgás és az egyéb biztonsági kockázatok kétségei vannak könnyen toosee.</span><span class="sxs-lookup"><span data-stu-id="723c0-107">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> <span data-ttu-id="723c0-108">Tájékoztatási hiánya teheti a felsorolt biztonsági kockázatok kezelésével tűnhet terv létrehozása.</span><span class="sxs-lookup"><span data-stu-id="723c0-108">This lack of awareness can make creating a plan for dealing with these security risks seem daunting.</span></span>

<span data-ttu-id="723c0-109">A cloud App Discovery lehetővé teszi az Azure Active Directory (AD) támogatás, amely lehetővé teszi a szervezet hello dolgozói által használt toodiscover felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="723c0-109">Cloud App Discovery is a feature of Azure Active Directory (AD) Premium that enables you toodiscover cloud applications being used by hello people in your organization.</span></span>

<span data-ttu-id="723c0-110">**A Cloud App Discovery a következőket teheti:**</span><span class="sxs-lookup"><span data-stu-id="723c0-110">**With Cloud App Discovery, you can:**</span></span>

* <span data-ttu-id="723c0-111">Keresse meg használt hello felhőalapú alkalmazásokhoz, és mérheti, hogy a használatát azon felhasználók számát, a forgalom vagy a kérelmek toohello webalkalmazás száma.</span><span class="sxs-lookup"><span data-stu-id="723c0-111">Find hello cloud applications being used and measure that usage by number of users, volume of traffic or number of web requests toohello application.</span></span>
* <span data-ttu-id="723c0-112">Az alkalmazás által használt hello felhasználók azonosítása.</span><span class="sxs-lookup"><span data-stu-id="723c0-112">Identify hello users that are using an application.</span></span>
* <span data-ttu-id="723c0-113">Exportálhatja az adatokat kapcsolat nélküli elemzéshez.</span><span class="sxs-lookup"><span data-stu-id="723c0-113">Export data for offline analysis.</span></span>
* <span data-ttu-id="723c0-114">Ezek az alkalmazások informatikai vezérlése alatt állapotba, és engedélyezze a egyszeri bejelentkezéshez a felhasználókezeléshez.</span><span class="sxs-lookup"><span data-stu-id="723c0-114">Bring these applications under IT control and enable single sign on for user management.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="723c0-115">Működés</span><span class="sxs-lookup"><span data-stu-id="723c0-115">How it works</span></span>
1. <span data-ttu-id="723c0-116">Alkalmazás használati ügynök telepítve van a felhasználók számítógépeire.</span><span class="sxs-lookup"><span data-stu-id="723c0-116">Application usage agents are installed on user's computers.</span></span>
2. <span data-ttu-id="723c0-117">hello alkalmazás használati adatai hello-ügynökök által rögzített egy biztonságos, titkosított csatornát felderítési toohello felhőalapú alkalmazásszolgáltatás továbbítása.</span><span class="sxs-lookup"><span data-stu-id="723c0-117">hello application usage information captured by hello agents is sent over a secure, encrypted channel toohello cloud app discovery service.</span></span>
3. <span data-ttu-id="723c0-118">a Cloud App Discovery szolgáltatás hello hello adatok kiértékeli, és létrehozza a jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="723c0-118">hello Cloud App Discovery service evaluates hello data and generates reports.</span></span>

![Cloud App Discovery diagramja](./media/active-directory-cloudappdiscovery/cad01.png)

<span data-ttu-id="723c0-120">lépések a Cloud App Discovery tooget lásd [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span><span class="sxs-lookup"><span data-stu-id="723c0-120">tooget started with Cloud App Discovery, see [Getting Started With Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span></span>

## <a name="related-articles"></a><span data-ttu-id="723c0-121">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="723c0-121">Related articles</span></span>
* [<span data-ttu-id="723c0-122">Cloud App Discovery biztonsági és adatvédelmi megfontolások</span><span class="sxs-lookup"><span data-stu-id="723c0-122">Cloud App Discovery Security and Privacy Considerations</span></span>](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [<span data-ttu-id="723c0-123">Cloud App Discovery csoport házirendjének telepítési útmutatója</span><span class="sxs-lookup"><span data-stu-id="723c0-123">Cloud App Discovery Group Policy Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [<span data-ttu-id="723c0-124">Cloud App Discovery a System Center telepítési útmutatója</span><span class="sxs-lookup"><span data-stu-id="723c0-124">Cloud App Discovery System Center Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [<span data-ttu-id="723c0-125">Cloud App Discovery beállításjegyzék-beállítások egyéni porttal proxykiszolgálók</span><span class="sxs-lookup"><span data-stu-id="723c0-125">Cloud App Discovery Registry Settings for Proxy Servers with Custom Ports</span></span>](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [<span data-ttu-id="723c0-126">Cloud App Discovery-ügynök változásnaplója</span><span class="sxs-lookup"><span data-stu-id="723c0-126">Cloud App Discovery Agent Changelog </span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [<span data-ttu-id="723c0-127">A cloud App Discovery gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="723c0-127">Cloud App Discovery Frequently Asked Questions</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [<span data-ttu-id="723c0-128">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="723c0-128">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

