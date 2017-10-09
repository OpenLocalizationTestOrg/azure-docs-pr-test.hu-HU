---
title: "aaaGet lépések az Azure MFA hello felhőben |} Microsoft Docs"
description: "Ez a hello Microsoft Azure multi-factor Authentication hitelesítés lap, amely leírja, hogyan tooget el az Azure MFA felhőben hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: kgremban
ms.openlocfilehash: a4aaf44bf08d96f2baad155072fdd6e0e727ce8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-hello-cloud"></a><span data-ttu-id="f53a2-103">Ismerkedés az Azure multi-factor Authentication hello felhőben</span><span class="sxs-lookup"><span data-stu-id="f53a2-103">Getting started with Azure Multi-Factor Authentication in hello cloud</span></span>
<span data-ttu-id="f53a2-104">Ez a cikk végigvezeti a tooget indítási módjának hello felhőalapú Azure multi-factor Authentication használatát.</span><span class="sxs-lookup"><span data-stu-id="f53a2-104">This article walks through how tooget started using Azure Multi-Factor Authentication in hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="f53a2-105">hello következő dokumentációja bemutatja, hogyan tooenable felhasználók hello **klasszikus Azure portál**.</span><span class="sxs-lookup"><span data-stu-id="f53a2-105">hello following documentation provides information on how tooenable users using hello **Azure Classic Portal**.</span></span> <span data-ttu-id="f53a2-106">Ha a keresett információk tooset be Azure multi-factor Authentication a felhasználók Office 365, lásd: [az Office 365 többtényezős hitelesítés beállítása.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span><span class="sxs-lookup"><span data-stu-id="f53a2-106">If you are looking for information on how tooset up Azure Multi-Factor Authentication for O365 users, see [Set up multi-factor authentication for Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span></span>

![A felhő hello MFA](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a><span data-ttu-id="f53a2-108">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="f53a2-108">Prerequisite</span></span>
<span data-ttu-id="f53a2-109">[Regisztráljon egy Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/) -még nem rendelkezik Azure-előfizetéssel, ha egy toosign felfelé kell.</span><span class="sxs-lookup"><span data-stu-id="f53a2-109">[Sign up for an Azure subscription](https://azure.microsoft.com/pricing/free-trial/) - If you do not already have an Azure subscription, you need toosign-up for one.</span></span> <span data-ttu-id="f53a2-110">Ha csak most kezdi és az Azure MFA-t használja, használhat próbaelőfizetést</span><span class="sxs-lookup"><span data-stu-id="f53a2-110">If you are just starting out and using Azure MFA you can use a trial subscription</span></span>

## <a name="enable-azure-multi-factor-authentication"></a><span data-ttu-id="f53a2-111">Az Azure Multi-Factor Authentication engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f53a2-111">Enable Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="f53a2-112">Mindaddig, amíg a felhasználók rendelkeznek licenccel, amely tartalmazza az Azure multi-factor Authentication, nincs szükség, hogy kell-e az Azure MFA toodo tooturn.</span><span class="sxs-lookup"><span data-stu-id="f53a2-112">As long as your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="f53a2-113">Felhasználói alapon, egyenként kezdeményezheti a kétlépéses ellenőrzés igénylését.</span><span class="sxs-lookup"><span data-stu-id="f53a2-113">You can start requiring two-step verification on an individual user basis.</span></span> <span data-ttu-id="f53a2-114">hello olyan licencet, amely engedélyezi az Azure MFA Használatát a következők:</span><span class="sxs-lookup"><span data-stu-id="f53a2-114">hello licenses that enable Azure MFA are:</span></span>
- <span data-ttu-id="f53a2-115">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="f53a2-115">Azure Multi-Factor Authentication</span></span>
- <span data-ttu-id="f53a2-116">Prémium szintű Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f53a2-116">Azure Active Directory Premium</span></span>
- <span data-ttu-id="f53a2-117">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="f53a2-117">Enterprise Mobility + Security</span></span>

<span data-ttu-id="f53a2-118">Ha még nincs fiókja, ezek három licencet, vagy nem rendelkezik elegendő licencek toocover minden felhasználó, közül túl ok.</span><span class="sxs-lookup"><span data-stu-id="f53a2-118">If you don't have one of these three licenses, or you don't have enough licenses toocover all of your users, that's ok too.</span></span> <span data-ttu-id="f53a2-119">Csak akkor toodo egy további lépést és [a többtényezős hitelesítésszolgáltató létrehozása](multi-factor-authentication-get-started-auth-provider.md) a címtárban.</span><span class="sxs-lookup"><span data-stu-id="f53a2-119">You just have toodo an extra step and [Create a Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md) in your directory.</span></span>

## <a name="turn-on-two-step-verification-for-users"></a><span data-ttu-id="f53a2-120">A kétlépéses ellenőrzés bekapcsolása a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="f53a2-120">Turn on two-step verification for users</span></span>

<span data-ttu-id="f53a2-121">A felsorolt hello eljárásokkal [hogyan toorequire kétlépéses ellenőrzés egy felhasználó vagy csoport](multi-factor-authentication-get-started-user-states.md) toostart az Azure MFA használatával.</span><span class="sxs-lookup"><span data-stu-id="f53a2-121">Use one of hello procedures listed in [How toorequire two-step verification for a user or group](multi-factor-authentication-get-started-user-states.md) toostart using Azure MFA.</span></span> <span data-ttu-id="f53a2-122">Kiválaszthatja a összes bejelentkezések tooenforce kétlépéses ellenőrzést, vagy csak akkor, ha akkor számít, tooyou hozhat létre feltételes hozzáférési házirendek toorequire kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="f53a2-122">You can choose tooenforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when it matters tooyou.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f53a2-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f53a2-123">Next Steps</span></span>
<span data-ttu-id="f53a2-124">Most, hogy állított be Azure multi-factor Authentication hello felhőben, konfigurálása, és állítsa be a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="f53a2-124">Now that you have set up Azure Multi-Factor Authentication in hello cloud, you can configure and set up your deployment.</span></span> <span data-ttu-id="f53a2-125">A részletekről lásd: [Az Azure Multi-Factor Authentication konfigurálása](multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="f53a2-125">See [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) for more details.</span></span>

