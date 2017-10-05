---
title: "Nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery keresése |} Microsoft Docs"
description: "Információk keresése és alkalmazások kezelése az a Cloud App Discovery, milyen előnyökkel és annak működéséről."
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
ms.openlocfilehash: 6284ff5bac8edbc19561d0916adef153526dfbe3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a><span data-ttu-id="b1ed8-104">A Cloud App Discovery megállapítás nem felügyelt felhőalkalmazások</span><span class="sxs-lookup"><span data-stu-id="b1ed8-104">Finding unmanaged cloud applications with Cloud App Discovery</span></span>
## <a name="overview"></a><span data-ttu-id="b1ed8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b1ed8-105">Overview</span></span>
<span data-ttu-id="b1ed8-106">A modern vállalatok informatikai részlegek ismerik gyakran nem a szervezet tagjai segítségével végezhesse a munkáját felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-106">In modern enterprises, IT departments are often not aware of all the cloud applications that members of their organization use to do their work.</span></span> <span data-ttu-id="b1ed8-107">Legyen könnyen látható, hogy miért a rendszergazdák jogosulatlan hozzáférés a vállalati adatokat, lehetséges adatszivárgás és egyéb biztonsági kockázatok kétségei vannak.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-107">It is easy to see why administrators would have concerns about unauthorized access to corporate data, possible data leakage and other security risks.</span></span> <span data-ttu-id="b1ed8-108">Tájékoztatási hiánya teheti a felsorolt biztonsági kockázatok kezelésével tűnhet terv létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-108">This lack of awareness can make creating a plan for dealing with these security risks seem daunting.</span></span>

<span data-ttu-id="b1ed8-109">A cloud App Discovery lehetővé teszi az Azure Active Directory (AD) támogatás, amely lehetővé teszi, hogy a szervezet dolgozói által használt felhőalkalmazások felderítése.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-109">Cloud App Discovery is a feature of Azure Active Directory (AD) Premium that enables you to discover cloud applications being used by the people in your organization.</span></span>

<span data-ttu-id="b1ed8-110">**A Cloud App Discovery a következőket teheti:**</span><span class="sxs-lookup"><span data-stu-id="b1ed8-110">**With Cloud App Discovery, you can:**</span></span>

* <span data-ttu-id="b1ed8-111">Keresse meg a felhőalapú alkalmazások használják, és mérheti a használatot felhasználók száma, a forgalom vagy a webkiszolgáló az alkalmazásnak küldött kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-111">Find the cloud applications being used and measure that usage by number of users, volume of traffic or number of web requests to the application.</span></span>
* <span data-ttu-id="b1ed8-112">Azonosítsa az adott alkalmazást használó felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-112">Identify the users that are using an application.</span></span>
* <span data-ttu-id="b1ed8-113">Exportálhatja az adatokat kapcsolat nélküli elemzéshez.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-113">Export data for offline analysis.</span></span>
* <span data-ttu-id="b1ed8-114">Ezek az alkalmazások informatikai vezérlése alatt állapotba, és engedélyezze a egyszeri bejelentkezéshez a felhasználókezeléshez.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-114">Bring these applications under IT control and enable single sign on for user management.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="b1ed8-115">Működés</span><span class="sxs-lookup"><span data-stu-id="b1ed8-115">How it works</span></span>
1. <span data-ttu-id="b1ed8-116">Alkalmazás használati ügynök telepítve van a felhasználók számítógépeire.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-116">Application usage agents are installed on user's computers.</span></span>
2. <span data-ttu-id="b1ed8-117">Az alkalmazás használati adatait, az ügynökök által rögzített a cloud app discovery szolgáltatásba zajlik a biztonságos, titkosított csatornán.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-117">The application usage information captured by the agents is sent over a secure, encrypted channel to the cloud app discovery service.</span></span>
3. <span data-ttu-id="b1ed8-118">A Cloud App Discovery szolgáltatásba kiértékeli az adatokat, és létrehozza a jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="b1ed8-118">The Cloud App Discovery service evaluates the data and generates reports.</span></span>

![Cloud App Discovery diagramja](./media/active-directory-cloudappdiscovery/cad01.png)

<span data-ttu-id="b1ed8-120">Első lépések a Cloud App Discovery, lásd: [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span><span class="sxs-lookup"><span data-stu-id="b1ed8-120">To get started with Cloud App Discovery, see [Getting Started With Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)</span></span>

## <a name="related-articles"></a><span data-ttu-id="b1ed8-121">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="b1ed8-121">Related articles</span></span>
* [<span data-ttu-id="b1ed8-122">Cloud App Discovery biztonsági és adatvédelmi megfontolások</span><span class="sxs-lookup"><span data-stu-id="b1ed8-122">Cloud App Discovery Security and Privacy Considerations</span></span>](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [<span data-ttu-id="b1ed8-123">Cloud App Discovery csoport házirendjének telepítési útmutatója</span><span class="sxs-lookup"><span data-stu-id="b1ed8-123">Cloud App Discovery Group Policy Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [<span data-ttu-id="b1ed8-124">Cloud App Discovery a System Center telepítési útmutatója</span><span class="sxs-lookup"><span data-stu-id="b1ed8-124">Cloud App Discovery System Center Deployment Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [<span data-ttu-id="b1ed8-125">Cloud App Discovery beállításjegyzék-beállítások egyéni porttal proxykiszolgálók</span><span class="sxs-lookup"><span data-stu-id="b1ed8-125">Cloud App Discovery Registry Settings for Proxy Servers with Custom Ports</span></span>](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [<span data-ttu-id="b1ed8-126">Cloud App Discovery-ügynök változásnaplója</span><span class="sxs-lookup"><span data-stu-id="b1ed8-126">Cloud App Discovery Agent Changelog </span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [<span data-ttu-id="b1ed8-127">A cloud App Discovery gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="b1ed8-127">Cloud App Discovery Frequently Asked Questions</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [<span data-ttu-id="b1ed8-128">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="b1ed8-128">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

