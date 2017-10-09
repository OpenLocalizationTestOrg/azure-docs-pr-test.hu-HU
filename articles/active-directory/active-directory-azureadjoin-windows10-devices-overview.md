---
title: "Windows 10 Enterprise hello: módon toouse eszközök munkára |} Microsoft Docs"
description: "A vállalatok számára, és hogyan hello az Azure Active Directoryval toointegrate Windows felhőalapú Windows 10-eszközökre történő telepítésének áttekintése. Hello különböző módokon eszköz kiépítése, és a vállalat hello Azure-portálon keresztül használt ellentéte."
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
ms.openlocfilehash: 95b452bc5ba3937e16de769275a59c77cb821e23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-10-for-hello-enterprise-ways-toouse-devices-for-work"></a><span data-ttu-id="2ce5f-105">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="2ce5f-105">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>
<span data-ttu-id="2ce5f-106">Windows 10 ad meg hello képességét tooleverage Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2ce5f-106">Windows 10 gives you hello ability tooleverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2ce5f-107">Windows 10-es eszközök tooAzure AD, hogy a felhasználók bejelentkezhetnek tooWindows Azure AD-fiókok használatával, vagy adja hozzá az Azure-azonosítók toogain hozzáférés toobusiness alkalmazásokat és erőforrásokat is elérheti.</span><span class="sxs-lookup"><span data-stu-id="2ce5f-107">You can connect Windows 10 devices tooAzure AD so that users can sign in tooWindows by using Azure AD accounts or by adding their Azure IDs toogain access toobusiness apps and resources.</span></span>

![A felhőalapú Windows Azure Active Directoryban](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="2ce5f-109">Windows 10-eszközök integrálása az Azure Active Directory – a tartalmak térképét</span><span class="sxs-lookup"><span data-stu-id="2ce5f-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="2ce5f-110">a következő témakörök hello betekintést különböző képességeket, a Windows 10-eszközök a szervezetben.</span><span class="sxs-lookup"><span data-stu-id="2ce5f-110">hello following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="2ce5f-111">Témakörök</span><span class="sxs-lookup"><span data-stu-id="2ce5f-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="2ce5f-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="2ce5f-112">Getting started</span></span> |[<span data-ttu-id="2ce5f-113">Windows 10-eszközök használatát a munkahelyi</span><span class="sxs-lookup"><span data-stu-id="2ce5f-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="2ce5f-114">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="2ce5f-114">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="2ce5f-115">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="2ce5f-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="2ce5f-116">Környezet</span><span class="sxs-lookup"><span data-stu-id="2ce5f-116">Deployment</span></span> |[<span data-ttu-id="2ce5f-117">Használati forgatókönyvek és az Azure AD-csatlakozás telepítési szempontjai</span><span class="sxs-lookup"><span data-stu-id="2ce5f-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="2ce5f-118">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD, a Windows 10 során lép fel.</span><span class="sxs-lookup"><span data-stu-id="2ce5f-118">Connecting domain-joined devices tooAzure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="2ce5f-119">Lehetővé teszi a Microsoft Passport for work hello szervezet</span><span class="sxs-lookup"><span data-stu-id="2ce5f-119">Enabling Microsoft Passport for work in hello organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="2ce5f-120">Engedélyezi a vállalati Állapothordozás a Windows 10 rendszerhez</span><span class="sxs-lookup"><span data-stu-id="2ce5f-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="2ce5f-121">Felhasználói feladatok</span><span class="sxs-lookup"><span data-stu-id="2ce5f-121">User tasks</span></span> |[<span data-ttu-id="2ce5f-122">A telepítés során az Azure AD-val Windows 10 új eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="2ce5f-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="2ce5f-123">Az Azure ad-val hello-beállítások menüjében Windows 10-eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="2ce5f-123">Setting up a Windows 10 device with Azure AD from hello settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="2ce5f-124">A Windows 10-es eszköz személyes tooyour szervezethez csatlakozó</span><span class="sxs-lookup"><span data-stu-id="2ce5f-124">Joining a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md) |

