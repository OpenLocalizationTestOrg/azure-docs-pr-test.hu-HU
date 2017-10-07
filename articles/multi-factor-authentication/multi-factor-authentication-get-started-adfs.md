---
title: "ellenőrzési lépés aaaTwo és az AD FS - Azure MFA |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget lépések az Azure MFA és az AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 7c1c925039d3cb753ba60e286168e5869faeae4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a><span data-ttu-id="ac2c2-103">Bevezetés az Azure Multi-Factor Authentication és az Active Directory összevonási szolgáltatások használatába</span><span class="sxs-lookup"><span data-stu-id="ac2c2-103">Getting started with Azure Multi-Factor Authentication and Active Directory Federation Services</span></span>
<span data-ttu-id="ac2c2-104"><center>![Felhő](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span><span class="sxs-lookup"><span data-stu-id="ac2c2-104"><center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span></span>

<span data-ttu-id="ac2c2-105">Ha a szervezete az AD FS használatával vonta össze a helyszíni Active Directoryt az Azure Active Directoryval, az Azure Multi-Factor Authentication két módon használható .</span><span class="sxs-lookup"><span data-stu-id="ac2c2-105">If your organization has federated your on-premises Active Directory with Azure Active Directory using AD FS, there are two options for using Azure Multi-Factor Authentication.</span></span>

* <span data-ttu-id="ac2c2-106">A felhőerőforrások védelme az Azure Multi-Factor Authentication vagy az Active Directory összevonási szolgáltatások használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-106">Secure cloud resources using Azure Multi-Factor Authentication or Active Directory Federation Services</span></span>
* <span data-ttu-id="ac2c2-107">A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-107">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server</span></span>

<span data-ttu-id="ac2c2-108">a következő táblázat hello hello ellenőrzési élmény biztonságossá tétele az Azure többtényezős hitelesítés és az AD FS a források között foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-108">hello following table summarizes hello verification experience between securing resources with Azure Multi-Factor Authentication and AD FS</span></span>

| <span data-ttu-id="ac2c2-109">Ellenőrzés – Böngészőalapú alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ac2c2-109">Verification Experience - Browser-based Apps</span></span> | <span data-ttu-id="ac2c2-110">Ellenőrzés – Nem böngészőalapú alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ac2c2-110">Verification Experience - Non-Browser-based Apps</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="ac2c2-111">Az Azure AD-erőforrások védelme az Azure Multi-Factor Authentication használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-111">Securing Azure AD resources using Azure Multi-Factor Authentication</span></span> |<li><span data-ttu-id="ac2c2-112">hello első ellenőrzési lépés történik a helyszíni Active Directory összevonási szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-112">hello first verification step is performed on-premises using AD FS.</span></span></li> <li><span data-ttu-id="ac2c2-113">hello második lépése a felhőalapú hitelesítés használatával végzett phone-alapú módszer.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-113">hello second step is a phone-based method carried out using cloud authentication.</span></span></li> |
| <span data-ttu-id="ac2c2-114">Az Azure AD-erőforrások védelme az Active Directory összevonási szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="ac2c2-114">Securing Azure AD resources using Active Directory Federation Services</span></span> |<li><span data-ttu-id="ac2c2-115">hello első ellenőrzési lépés történik a helyszíni Active Directory összevonási szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-115">hello first verification step is performed on-premises using AD FS.</span></span></li><li><span data-ttu-id="ac2c2-116">hello második lépésben szerint hello jogcím érvényesítenie a helyszínen történik.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-116">hello second step is performed on-premises by honoring hello claim.</span></span></li> |

<span data-ttu-id="ac2c2-117">Összevont felhasználók alkalmazásjelszavaival kapcsolatos figyelmeztetések:</span><span class="sxs-lookup"><span data-stu-id="ac2c2-117">Caveats with app passwords for federated users:</span></span>

* <span data-ttu-id="ac2c2-118">Az alkalmazásjelszavak ellenőrzése felhőalapú hitelesítéssel történik, így mellőzik az összevonásokat.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-118">App passwords are verified using cloud authentication, so they bypass federation.</span></span> <span data-ttu-id="ac2c2-119">Az összevonás csak alkalmazásjelszó beállításakor van aktív használatban.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-119">Federation is only actively used when setting up an app password.</span></span>
* <span data-ttu-id="ac2c2-120">Az alkalmazásjelszavak nem tartják be a helyszíni ügyfél hozzáférés-vezérlési beállításait.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-120">On-premises Client Access Control settings are not honored by app passwords.</span></span>
* <span data-ttu-id="ac2c2-121">Az alkalmazásjelszavak használata esetén nem érhető el a helyszíni hitelesítésnaplózás.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-121">You lose on-premises authentication-logging capability for app passwords.</span></span>
* <span data-ttu-id="ac2c2-122">Fiók letiltása/törlése címtár-Szinkronizáló letiltását/törlését a hello felhőalapú identitás alkalmazásjelszók késleltetése toothree órát igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="ac2c2-122">Account disable/deletion may take up toothree hours for directory sync, delaying disable/deletion of app passwords in hello cloud identity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac2c2-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac2c2-123">Next steps</span></span>
<span data-ttu-id="ac2c2-124">Azure multi-factor Authentication vagy hello Azure multi-factor Authentication kiszolgáló az AD FS beállításával kapcsolatos információkért lásd: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="ac2c2-124">For information on setting up either Azure Multi-Factor Authentication or hello Azure Multi-Factor Authentication Server with AD FS, see hello following articles:</span></span>

* [<span data-ttu-id="ac2c2-125">A felhőerőforrások védelme az Azure Multi-Factor Authentication és az AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-125">Secure cloud resources using Azure Multi-Factor Authentication and AD FS</span></span>](multi-factor-authentication-get-started-adfs-cloud.md)
* [<span data-ttu-id="ac2c2-126">A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló és a Windows Server 2012 R2 AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-126">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with Windows Server 2012 R2 AD FS</span></span>](multi-factor-authentication-get-started-adfs-w2k12.md)
* [<span data-ttu-id="ac2c2-127">A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló és az AD FS 2.0-s verziójának használatával</span><span class="sxs-lookup"><span data-stu-id="ac2c2-127">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with AD FS 2.0</span></span>](multi-factor-authentication-get-started-adfs-adfs2.md)
