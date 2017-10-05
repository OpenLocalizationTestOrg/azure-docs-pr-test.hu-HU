---
title: "Bevezetés az Azure MFA üzemeltetésébe a felhőben |Microsoft Docs"
description: "Ez a Microsoft Azure Multi-Factor Authentication-oldal leírja, hogyan kezdheti el az Azure MFA használatát a felhőben."
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
ms.openlocfilehash: 19f3228b874fc4e37bf83388dae4341428226482
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a><span data-ttu-id="39af8-103">Azure Multi-Factor Authentication a felhőben – első lépések</span><span class="sxs-lookup"><span data-stu-id="39af8-103">Getting started with Azure Multi-Factor Authentication in the cloud</span></span>
<span data-ttu-id="39af8-104">A cikk végigkalauzolja az Azure Multi-Factor Authentication a felhőben való használatának kezdeti lépésein.</span><span class="sxs-lookup"><span data-stu-id="39af8-104">This article walks through how to get started using Azure Multi-Factor Authentication in the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="39af8-105">A következő dokumentáció arról nyújt tájékoztatást, hogyan engedélyezhet felhasználókat a **klasszikus Azure portállal**.</span><span class="sxs-lookup"><span data-stu-id="39af8-105">The following documentation provides information on how to enable users using the **Azure Classic Portal**.</span></span> <span data-ttu-id="39af8-106">Az Azure Multi-Factor Authentication az O365-felhasználók számára való beállításával kapcsolatos információk: [Többtényezős hitelesítés beállítása az Office 365-höz.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span><span class="sxs-lookup"><span data-stu-id="39af8-106">If you are looking for information on how to set up Azure Multi-Factor Authentication for O365 users, see [Set up multi-factor authentication for Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span></span>

![MFA a felhőben](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a><span data-ttu-id="39af8-108">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="39af8-108">Prerequisite</span></span>
<span data-ttu-id="39af8-109">[Regisztráció Azure-előfizetésre](https://azure.microsoft.com/pricing/free-trial/) – Ha még nincs Azure-előfizetése, regisztrálnia kell rá.</span><span class="sxs-lookup"><span data-stu-id="39af8-109">[Sign up for an Azure subscription](https://azure.microsoft.com/pricing/free-trial/) - If you do not already have an Azure subscription, you need to sign-up for one.</span></span> <span data-ttu-id="39af8-110">Ha csak most kezdi és az Azure MFA-t használja, használhat próbaelőfizetést</span><span class="sxs-lookup"><span data-stu-id="39af8-110">If you are just starting out and using Azure MFA you can use a trial subscription</span></span>

## <a name="enable-azure-multi-factor-authentication"></a><span data-ttu-id="39af8-111">Az Azure Multi-Factor Authentication engedélyezése</span><span class="sxs-lookup"><span data-stu-id="39af8-111">Enable Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="39af8-112">Ha a felhasználói rendelkeznek az Azure Multi-Factor Authenticationt magában foglaló licenccel, semmit nem kell tennie az Azure MFA bekapcsolásához.</span><span class="sxs-lookup"><span data-stu-id="39af8-112">As long as your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need to do to turn on Azure MFA.</span></span> <span data-ttu-id="39af8-113">Felhasználói alapon, egyenként kezdeményezheti a kétlépéses ellenőrzés igénylését.</span><span class="sxs-lookup"><span data-stu-id="39af8-113">You can start requiring two-step verification on an individual user basis.</span></span> <span data-ttu-id="39af8-114">Az Azure MFA használatát lehetővé tévő licencek:</span><span class="sxs-lookup"><span data-stu-id="39af8-114">The licenses that enable Azure MFA are:</span></span>
- <span data-ttu-id="39af8-115">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="39af8-115">Azure Multi-Factor Authentication</span></span>
- <span data-ttu-id="39af8-116">Prémium szintű Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39af8-116">Azure Active Directory Premium</span></span>
- <span data-ttu-id="39af8-117">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="39af8-117">Enterprise Mobility + Security</span></span>

<span data-ttu-id="39af8-118">Ha nem rendelkezik ilyen licencekkel, vagy nincs elég licence az összes felhasználó lefedéséhez, az sem gond.</span><span class="sxs-lookup"><span data-stu-id="39af8-118">If you don't have one of these three licenses, or you don't have enough licenses to cover all of your users, that's ok too.</span></span> <span data-ttu-id="39af8-119">Csak egy plusz lépésre van szüksége, hogy [létrehozzon egy többtényezős hitelesítési szolgáltatót](multi-factor-authentication-get-started-auth-provider.md) a könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="39af8-119">You just have to do an extra step and [Create a Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md) in your directory.</span></span>

## <a name="turn-on-two-step-verification-for-users"></a><span data-ttu-id="39af8-120">A kétlépéses ellenőrzés bekapcsolása a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="39af8-120">Turn on two-step verification for users</span></span>

<span data-ttu-id="39af8-121">Az Azure MFA használatának megkezdéséhez használja a [kétlépéses ellenőrzés felhasználók vagy csoportok számára történő megkövetelését](multi-factor-authentication-get-started-user-states.md) ismertető cikkben felsorolt eljárások valamelyikét.</span><span class="sxs-lookup"><span data-stu-id="39af8-121">Use one of the procedures listed in [How to require two-step verification for a user or group](multi-factor-authentication-get-started-user-states.md) to start using Azure MFA.</span></span> <span data-ttu-id="39af8-122">Választhat, hogy kétlépéses ellenőrzést követel meg az összes bejelentkezéshez, vagy olyan feltételes hozzáférési szabályzatokat hoz létre, amelyek csak olyan esetekben követelnek meg kétlépéses ellenőrzést, amelyekben annak szükségét érzi.</span><span class="sxs-lookup"><span data-stu-id="39af8-122">You can choose to enforce two-step verification for all sign-ins, or you can create conditional access policies to require two-step verification only when it matters to you.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39af8-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39af8-123">Next Steps</span></span>
<span data-ttu-id="39af8-124">Most, hogy beállította az Azure Multi-Factor Authenticationt a felhőben, konfigurálhatja és beállíthatja az üzemelő példányt.</span><span class="sxs-lookup"><span data-stu-id="39af8-124">Now that you have set up Azure Multi-Factor Authentication in the cloud, you can configure and set up your deployment.</span></span> <span data-ttu-id="39af8-125">A részletekről lásd: [Az Azure Multi-Factor Authentication konfigurálása](multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="39af8-125">See [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) for more details.</span></span>

