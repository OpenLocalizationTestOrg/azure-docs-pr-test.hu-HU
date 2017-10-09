---
title: "a felhasználók fel az Azure AD Join aaaSetting |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogy a rendszergazdák miként állíthatják be az Azure AD Joint a helyszíni címtár- és eszközregisztrációhoz."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a><span data-ttu-id="1e6c1-103">Az Azure AD Join beállítása a vállalatánál</span><span class="sxs-lookup"><span data-stu-id="1e6c1-103">Setting up Azure AD Join in your organization</span></span>
<span data-ttu-id="1e6c1-104">Az Azure Active Directory Join (Azure AD Join) beállítása előtt tooeither szinkronizálási kell regisztrálnia a helyszíni címtár felhasználói toohello felhő, vagy manuálisan felügyelt fiókokat létrehoznia az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-104">Before you set up Azure Active Directory Join (Azure AD Join), you need tooeither sync up your on-premises directory of users toohello cloud or manually create managed accounts in Azure AD.</span></span>

<span data-ttu-id="1e6c1-105">Szinkronizálás a helyszíni felhasználók tooAzure AD ismertetett történő szinkronizálásának részletes útmutatásáért [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="1e6c1-105">Detailed instructions for syncing your on-premises users tooAzure AD is covered in [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="1e6c1-106">toomanually létrehozása és felhasználók kezelése az Azure ad-ben, olvassa el a túl[felhasználók kezelése az Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e6c1-106">toomanually create and manage users in Azure AD, refer too[User management in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span></span>

## <a name="set-up-device-registration"></a><span data-ttu-id="1e6c1-107">Az eszközregisztráció beállítása</span><span class="sxs-lookup"><span data-stu-id="1e6c1-107">Set up device registration</span></span>
1. <span data-ttu-id="1e6c1-108">Jelentkezzen be toohello Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-108">Log on toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="1e6c1-109">Hello bal oldali ablaktáblában jelölje ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-109">On hello left pane, select **Active Directory**.</span></span>
3. <span data-ttu-id="1e6c1-110">A hello **Directory** lapra, válassza ki azt a címtárat.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-110">On hello **Directory** tab, select your directory.</span></span>
4. <span data-ttu-id="1e6c1-111">Jelölje be hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-111">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="1e6c1-112">Nyissa meg toohello **eszközök** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-112">Go toohello **Devices** section.</span></span>
6. <span data-ttu-id="1e6c1-113">A hello **eszközök** lapján állítsa be a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="1e6c1-113">On hello **devices** tab, set hello following:</span></span>  
   * <span data-ttu-id="1e6c1-114">**MAXIMÁLIS SZÁMÁT az eszközök felhasználónként**: válassza ki, amelyeken az Azure ad-ben a felhasználói eszközök hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-114">**MAXIMUM NUMBER OF DEVICES PER USER**: Select hello maximum number of devices that a user can have in Azure AD.</span></span>  <span data-ttu-id="1e6c1-115">Ha egy felhasználó eléri ezt a kvótát, nem fogják tudni tooadd további eszközöket egy vagy több meglévő eszközt eltávolításáig.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-115">If a user reaches this quota, they will not be able tooadd additional devices until one or more of their existing devices are removed.</span></span>
   * <span data-ttu-id="1e6c1-116">**IGÉNYELNEK a TÖBBTÉNYEZŐS hitelesítés tooJOIN eszközök**: akár felhasználók szükség tooprovide beállítása egy második hitelesítési tényező toojoin az eszköz tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-116">**REQUIRE MULTI-FACTOR AUTH tooJOIN DEVICES**: Set whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="1e6c1-117">További információ az Azure multi-factor Authentication: [Ismerkedés az Azure multi-factor Authentication hello felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="1e6c1-117">For more information on Azure Multi-Factor Authentication, see [Getting started with Azure Multi-Factor Authentication in hello cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>
   * <span data-ttu-id="1e6c1-118">**FELHASZNÁLÓK előfordulhat, hogy az AZURE AD JOIN eszközök**: hello felhasználókat és csoportokat, amelyek toojoin eszközök tooAzure AD kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-118">**USERS MAY AZURE AD JOIN DEVICES**: Select hello users and groups that are allowed toojoin devices tooAzure AD.</span></span>
   * <span data-ttu-id="1e6c1-119">**További RENDSZERGAZDÁK az AZURE AD csatlakoztatott eszközök**: az Azure AD prémium vagy hello nagyvállalati mobilitási csomag (EMS), kiválaszthatja, mely felhasználók kapjanak helyi rendszergazdai jogosultságokkal toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-119">**ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES**: With Azure AD Premium or hello Enterprise Mobility Suite (EMS), you can choose which users are granted local administrator rights toohello device.</span></span> <span data-ttu-id="1e6c1-120">A globális rendszergazdák és eszköztulajdonosok alapértelmezés szerint helyi rendszergazdai jogosultságot kapnak.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-120">Global administrators and device owners are granted local administrator rights by default.</span></span>

<span data-ttu-id="1e6c1-121"><center>![Az eszközregisztráció beállítása](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center></span><span class="sxs-lookup"><span data-stu-id="1e6c1-121"><center>![Set up device regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span></span>

<span data-ttu-id="1e6c1-122">Beállítása után Azure AD Joint a felhasználók számára, hogy csatlakozhassanak tooAzure AD keresztül a vállalati vagy személyes eszközökön.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-122">After you set up Azure AD Join for your users, they can connect tooAzure AD through their corporate or personal devices.</span></span>

<span data-ttu-id="1e6c1-123">Az alábbiakban hello három forgatókönyvvel tooenable a felhasználók tooset mentése az Azure AD Join:</span><span class="sxs-lookup"><span data-stu-id="1e6c1-123">Following are hello three scenarios you can use tooenable your users tooset up Azure AD Join:</span></span>

* <span data-ttu-id="1e6c1-124">Felhasználók a vállalati tulajdonban lévő eszköz csatlakozzon közvetlenül tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-124">Users join a company-owned device directly tooAzure AD.</span></span>
* <span data-ttu-id="1e6c1-125">Felhasználók tartományhoz való csatlakozást a vállalati tulajdonú eszköz toohello a helyszíni Active Directory, majd kiterjeszthetik hello eszköz tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1e6c1-125">Users domain-join a company-owned device toohello on-premises Active Directory and then extend hello device tooAzure AD.</span></span>
* <span data-ttu-id="1e6c1-126">Felhasználók hozzáadása a munkahelyi vagy iskolai fiókok tooWindows személyes eszköz</span><span class="sxs-lookup"><span data-stu-id="1e6c1-126">Users add work or school accounts tooWindows on a personal device</span></span>

## <a name="additional-information"></a><span data-ttu-id="1e6c1-127">További információ</span><span class="sxs-lookup"><span data-stu-id="1e6c1-127">Additional information</span></span>
* [<span data-ttu-id="1e6c1-128">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="1e6c1-128">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="1e6c1-129">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="1e6c1-129">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="1e6c1-130">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="1e6c1-130">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="1e6c1-131">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="1e6c1-131">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="1e6c1-132">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="1e6c1-132">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

