---
title: "aaaAzure AD csatlakozás a Microsoft Cloud-Németország"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazásokhoz közös identitás tooprovide."
keywords: "Bevezetés tooAzure AD Connect, az Azure AD Connect áttekintése, mi az az Azure AD Connect, az active directory, Németország, fekete erdő telepítése"
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
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="b0dd3-105">Azure AD Connect a Microsoft Cloud németországi adatközpontjában – nyilvános előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="b0dd3-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="b0dd3-106">Introduction (Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="b0dd3-106">Introduction</span></span>
<span data-ttu-id="b0dd3-107">Az Azure AD Connect szinkronizálást biztosít a helyszíni Active Directory és az Azure Active Directory között.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="b0dd3-108">Jelenleg számos hello forgatókönyvei [Microsoft Cloud Németország](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) hello operátort kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-108">Currently, many of hello scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by hello operator.</span></span> <span data-ttu-id="b0dd3-109">A Microsoft Cloud Németország használatakor figyelembe kell venni hello alábbi:</span><span class="sxs-lookup"><span data-stu-id="b0dd3-109">When using Microsoft Cloud Germany, you must be aware of hello following:</span></span>

* <span data-ttu-id="b0dd3-110">következő URL-címeket kell megnyitni a szinkronizálási toooccur proxykiszolgáló sikeresen hello:</span><span class="sxs-lookup"><span data-stu-id="b0dd3-110">hello following URLs must be opened on a proxy server for synchronization toooccur successfully:</span></span>
  
  * <span data-ttu-id="b0dd3-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="b0dd3-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="b0dd3-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="b0dd3-112">*.windows.net</span></span>
  * * <span data-ttu-id="b0dd3-113">Visszavont tanúsítványok listái (CRL-ek)</span><span class="sxs-lookup"><span data-stu-id="b0dd3-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="b0dd3-114">Azure AD-címtár tooyour bejelentkezéskor hello onmicrosoft.de tartományban olyan fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-114">When you sign in tooyour Azure AD directory, you must use an account in hello onmicrosoft.de domain.</span></span>
* <span data-ttu-id="b0dd3-115">hello a következő szolgáltatások nem érhetők el:</span><span class="sxs-lookup"><span data-stu-id="b0dd3-115">hello following features are not available:</span></span>
  * <span data-ttu-id="b0dd3-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="b0dd3-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="b0dd3-117">Automatikus frissítések</span><span class="sxs-lookup"><span data-stu-id="b0dd3-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="b0dd3-118">Letöltés</span><span class="sxs-lookup"><span data-stu-id="b0dd3-118">Download</span></span>
<span data-ttu-id="b0dd3-119">Az Azure AD Connect letölthető hello Azure AD Connect panel hello portálon találja meg.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-119">You can download Azure AD Connect from hello Azure AD Connect blade within hello portal.</span></span>  <span data-ttu-id="b0dd3-120">Használja a hello utasításokat toolocate hello Azure AD Connect panel alatt.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-120">Use hello instructions below toolocate hello Azure AD Connect blade.</span></span>

### <a name="hello-azure-ad-connect-blade"></a><span data-ttu-id="b0dd3-121">hello Azure AD Connect panel</span><span class="sxs-lookup"><span data-stu-id="b0dd3-121">hello Azure AD Connect Blade</span></span>
<span data-ttu-id="b0dd3-122">Miután bejelentkezett az Azure-portálon toohello, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b0dd3-122">Once you have signed in toohello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="b0dd3-123">Nyissa meg tooBrowse</span><span class="sxs-lookup"><span data-stu-id="b0dd3-123">Go tooBrowse</span></span>
2. <span data-ttu-id="b0dd3-124">Válassza az Azure Active Directory elemet.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="b0dd3-125">Ezután válassza az Azure AD Connect elemet.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="b0dd3-126">Hello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="b0dd3-126">You should see hello following:</span></span>

![Az Azure AD Connect panel](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="b0dd3-128">hello következő táblázatban hello szolgáltatások hello panelen látható.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-128">hello following table describes hello features shown in hello blade.</span></span>

| <span data-ttu-id="b0dd3-129">Cím</span><span class="sxs-lookup"><span data-stu-id="b0dd3-129">Title</span></span> | <span data-ttu-id="b0dd3-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0dd3-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b0dd3-131">SZINKRONIZÁLÁS ÁLLAPOTA</span><span class="sxs-lookup"><span data-stu-id="b0dd3-131">SYNC STATUS</span></span> |<span data-ttu-id="b0dd3-132">Arról tájékoztatja, hogy a szinkronizálás engedélyezve van-e, vagy le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="b0dd3-133">LEGUTÓBBI SZINKRONIZÁLÁS</span><span class="sxs-lookup"><span data-stu-id="b0dd3-133">LAST SYNC</span></span> |<span data-ttu-id="b0dd3-134">hello legutóbbi sikeres szinkronizálás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-134">hello last time a successful sync completed.</span></span> |
| <span data-ttu-id="b0dd3-135">ÖSSZEVONT TARTOMÁNYOK</span><span class="sxs-lookup"><span data-stu-id="b0dd3-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="b0dd3-136">Jelenleg konfigurált összevont tartományt hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-136">Shows hello number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="b0dd3-137">Telepítés</span><span class="sxs-lookup"><span data-stu-id="b0dd3-137">Installation</span></span>
<span data-ttu-id="b0dd3-138">az Azure AD Connect tooinstall, hello dokumentáció használható [Itt](active-directory-aadconnect.md#install-azure-ad-connect).</span><span class="sxs-lookup"><span data-stu-id="b0dd3-138">tooinstall Azure AD Connect, you can use hello documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="b0dd3-139">Speciális szolgáltatások és további információk</span><span class="sxs-lookup"><span data-stu-id="b0dd3-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="b0dd3-140">Ha további tájékoztatást, illetve az egyéni beállításokkal vagy speciális konfigurációkkal kapcsolatos útmutatást keres, kezdje a [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md) című cikkel.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="b0dd3-141">Ezen a lapon jelenít meg adatokat és hivatkozásokat tooadditional útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b0dd3-141">This page provides information and links tooadditional guidance.</span></span>

