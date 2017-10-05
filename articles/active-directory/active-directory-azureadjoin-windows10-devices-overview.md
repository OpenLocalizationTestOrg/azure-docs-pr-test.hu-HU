---
title: "A vállalati Windows 10: eszközök használata munkára módjai |} Microsoft Docs"
description: "A vállalatok és a Windows felhő integrálása az Azure Active Directoryval Windows 10-eszközökre történő telepítésének áttekintése. Ellentétben a különböző módokon eszköz üzembe helyezve, és a vállalat az Azure portálon keresztül használt."
keywords: "a felhőalapú Windows Azure Active Directory, a Windows Azure, a Windows Azure-eszközök a Windows 10-eszközök"
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2cb9ab6a-55b6-4658-b7f2-6e05ae015e1b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 804156048a7596f9937098e6fe762f424526473c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-10-for-the-enterprise-ways-to-use-devices-for-work"></a><span data-ttu-id="e1495-105">Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata</span><span class="sxs-lookup"><span data-stu-id="e1495-105">Windows 10 for the enterprise: Ways to use devices for work</span></span>
<span data-ttu-id="e1495-106">Windows 10 lehetővé teszi az Azure Active Directory (Azure AD) ki.</span><span class="sxs-lookup"><span data-stu-id="e1495-106">Windows 10 gives you the ability to leverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e1495-107">Kapcsolódás a Windows 10-eszközökre az Azure AD, hogy a felhasználók való bejelentkezés a Windows Azure AD-fiókok használatával, vagy adja hozzá az Azure-azonosítók az üzleti alkalmazások és erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e1495-107">You can connect Windows 10 devices to Azure AD so that users can sign in to Windows by using Azure AD accounts or by adding their Azure IDs to gain access to business apps and resources.</span></span>

![A felhőalapú Windows Azure Active Directoryban](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="e1495-109">Windows 10-eszközök integrálása az Azure Active Directory – a tartalmak térképét</span><span class="sxs-lookup"><span data-stu-id="e1495-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="e1495-110">Az alábbi témakörök nyújtanak a szervezet Windows 10-eszközök különböző képességeket betekintést.</span><span class="sxs-lookup"><span data-stu-id="e1495-110">The following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="e1495-111">Témakörök</span><span class="sxs-lookup"><span data-stu-id="e1495-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="e1495-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e1495-112">Getting started</span></span> |[<span data-ttu-id="e1495-113">Windows 10-eszközök használatát a munkahelyi</span><span class="sxs-lookup"><span data-stu-id="e1495-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="e1495-114">A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül</span><span class="sxs-lookup"><span data-stu-id="e1495-114">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="e1495-115">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="e1495-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="e1495-116">Környezet</span><span class="sxs-lookup"><span data-stu-id="e1495-116">Deployment</span></span> |[<span data-ttu-id="e1495-117">Használati forgatókönyvek és az Azure AD-csatlakozás telepítési szempontjai</span><span class="sxs-lookup"><span data-stu-id="e1495-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="e1495-118">Tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD a Windows 10 során lép fel.</span><span class="sxs-lookup"><span data-stu-id="e1495-118">Connecting domain-joined devices to Azure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="e1495-119">Lehetővé teszi a Microsoft Passport for work a szervezet</span><span class="sxs-lookup"><span data-stu-id="e1495-119">Enabling Microsoft Passport for work in the organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="e1495-120">Engedélyezi a vállalati Állapothordozás a Windows 10 rendszerhez</span><span class="sxs-lookup"><span data-stu-id="e1495-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="e1495-121">Felhasználói feladatok</span><span class="sxs-lookup"><span data-stu-id="e1495-121">User tasks</span></span> |[<span data-ttu-id="e1495-122">A telepítés során az Azure AD-val Windows 10 új eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="e1495-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="e1495-123">A beállítások menüből a Windows 10-eszköz az Azure AD beállítása</span><span class="sxs-lookup"><span data-stu-id="e1495-123">Setting up a Windows 10 device with Azure AD from the settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="e1495-124">A szervezet egy személyes Windows 10 rendszerű eszköz csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e1495-124">Joining a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md) |

