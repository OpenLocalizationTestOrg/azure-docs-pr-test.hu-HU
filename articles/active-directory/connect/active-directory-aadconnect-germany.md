---
title: "Azure AD Connect a Microsoft Cloud németországi adatközpontjában"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Így közös identitást biztosíthat az Azure AD-vel integrált Office 365-, Azure- és SaaS-alkalmazásokhoz."
keywords: "Azure AD Connect bemutatása, Azure AD Connect áttekintése, mi az Azure AD Connect, active directory telepítése, Németország, Fekete-erdő"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4c55b116c0dc080ae316caca873a7b693caa793b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="37ee9-105">Azure AD Connect a Microsoft Cloud németországi adatközpontjában – nyilvános előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="37ee9-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="37ee9-106">Introduction (Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="37ee9-106">Introduction</span></span>
<span data-ttu-id="37ee9-107">Az Azure AD Connect szinkronizálást biztosít a helyszíni Active Directory és az Azure Active Directory között.</span><span class="sxs-lookup"><span data-stu-id="37ee9-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="37ee9-108">A [Microsoft Cloud németországi adatközpontja](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) esetében az eljárások jelentős részét a kezelőnek kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="37ee9-108">Currently, many of the scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by the operator.</span></span> <span data-ttu-id="37ee9-109">A Microsoft Cloud németországi adatközpontjának használata esetén a következőkre kell ügyelnie:</span><span class="sxs-lookup"><span data-stu-id="37ee9-109">When using Microsoft Cloud Germany, you must be aware of the following:</span></span>

* <span data-ttu-id="37ee9-110">A következő URL-címeket meg kell nyitni egy proxykiszolgálón ahhoz, hogy sikeres lehessen a szinkronizálás:</span><span class="sxs-lookup"><span data-stu-id="37ee9-110">The following URLs must be opened on a proxy server for synchronization to occur successfully:</span></span>
  
  * <span data-ttu-id="37ee9-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="37ee9-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="37ee9-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="37ee9-112">*.windows.net</span></span>
  * * <span data-ttu-id="37ee9-113">Visszavont tanúsítványok listái (CRL-ek)</span><span class="sxs-lookup"><span data-stu-id="37ee9-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="37ee9-114">Az Azure AD címtárba való bejelentkezéskor az onmicrosoft.de tartományba tartozó fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="37ee9-114">When you sign in to your Azure AD directory, you must use an account in the onmicrosoft.de domain.</span></span>
* <span data-ttu-id="37ee9-115">A következő funkciók nem érhetők el:</span><span class="sxs-lookup"><span data-stu-id="37ee9-115">The following features are not available:</span></span>
  * <span data-ttu-id="37ee9-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="37ee9-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="37ee9-117">Automatikus frissítések</span><span class="sxs-lookup"><span data-stu-id="37ee9-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="37ee9-118">Letöltés</span><span class="sxs-lookup"><span data-stu-id="37ee9-118">Download</span></span>
<span data-ttu-id="37ee9-119">Az Azure AD Connect a portál Azure AD Connect paneljéről tölthető le.</span><span class="sxs-lookup"><span data-stu-id="37ee9-119">You can download Azure AD Connect from the Azure AD Connect blade within the portal.</span></span>  <span data-ttu-id="37ee9-120">Az Azure AD Connect panelt az alábbi útmutatás alapján keresheti meg.</span><span class="sxs-lookup"><span data-stu-id="37ee9-120">Use the instructions below to locate the Azure AD Connect blade.</span></span>

### <a name="the-azure-ad-connect-blade"></a><span data-ttu-id="37ee9-121">Az Azure AD Connect panel</span><span class="sxs-lookup"><span data-stu-id="37ee9-121">The Azure AD Connect Blade</span></span>
<span data-ttu-id="37ee9-122">Miután bejelentkezett az Azure Portalra, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="37ee9-122">Once you have signed in to the Azure portal, do the following:</span></span>

1. <span data-ttu-id="37ee9-123">Válassza a Tallózás lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="37ee9-123">Go to Browse</span></span>
2. <span data-ttu-id="37ee9-124">Válassza az Azure Active Directory elemet.</span><span class="sxs-lookup"><span data-stu-id="37ee9-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="37ee9-125">Ezután válassza az Azure AD Connect elemet.</span><span class="sxs-lookup"><span data-stu-id="37ee9-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="37ee9-126">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="37ee9-126">You should see the following:</span></span>

![Az Azure AD Connect panel](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="37ee9-128">A következő táblázat a panelen látható funkciókat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="37ee9-128">The following table describes the features shown in the blade.</span></span>

| <span data-ttu-id="37ee9-129">Cím</span><span class="sxs-lookup"><span data-stu-id="37ee9-129">Title</span></span> | <span data-ttu-id="37ee9-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="37ee9-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="37ee9-131">SZINKRONIZÁLÁS ÁLLAPOTA</span><span class="sxs-lookup"><span data-stu-id="37ee9-131">SYNC STATUS</span></span> |<span data-ttu-id="37ee9-132">Arról tájékoztatja, hogy a szinkronizálás engedélyezve van-e, vagy le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="37ee9-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="37ee9-133">LEGUTÓBBI SZINKRONIZÁLÁS</span><span class="sxs-lookup"><span data-stu-id="37ee9-133">LAST SYNC</span></span> |<span data-ttu-id="37ee9-134">A legutóbbi alkalom, amikor sikeresen befejeződött egy szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="37ee9-134">The last time a successful sync completed.</span></span> |
| <span data-ttu-id="37ee9-135">ÖSSZEVONT TARTOMÁNYOK</span><span class="sxs-lookup"><span data-stu-id="37ee9-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="37ee9-136">Azt mutatja, hogy jelenleg hány összevont tartomány van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="37ee9-136">Shows the number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="37ee9-137">Telepítés</span><span class="sxs-lookup"><span data-stu-id="37ee9-137">Installation</span></span>
<span data-ttu-id="37ee9-138">Az Azure AD Connect telepítéséhez használhatja az [itt](active-directory-aadconnect.md#install-azure-ad-connect) található dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="37ee9-138">To install Azure AD Connect, you can use the documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="37ee9-139">Speciális szolgáltatások és további információk</span><span class="sxs-lookup"><span data-stu-id="37ee9-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="37ee9-140">Ha további tájékoztatást, illetve az egyéni beállításokkal vagy speciális konfigurációkkal kapcsolatos útmutatást keres, kezdje a [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md) című cikkel.</span><span class="sxs-lookup"><span data-stu-id="37ee9-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="37ee9-141">Ez a lap a további útmutatással kapcsolatos információkat és hivatkozásokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="37ee9-141">This page provides information and links to additional guidance.</span></span>

