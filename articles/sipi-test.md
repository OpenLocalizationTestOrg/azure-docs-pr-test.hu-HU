---
title: "Sipi fájl tesztelése |} Microsoft Docs"
description: "Toocheck ReadyForTest fájlfüggőségek tesztelése"
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: afd3dc94dfb30926b316256fb06a768a391004f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sipi-test-file"></a><span data-ttu-id="ef8f9-103">Sipi fájl tesztelése</span><span class="sxs-lookup"><span data-stu-id="ef8f9-103">Sipi test file</span></span>

<span data-ttu-id="ef8f9-104">Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="ef8f9-105">Amikor végzett, az alkalmazás regisztrálva van hello Azure B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-105">When you're finished, your application is registered for use in hello Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef8f9-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ef8f9-106">Prerequisites</span></span>

<span data-ttu-id="ef8f9-107">a felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás toobuild, először tooregister hello alkalmazás Azure Active Directory B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-107">toobuild an application that accepts consumer sign-up and sign-in, you first need tooregister hello application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="ef8f9-108">Című rész lépéseit hello segítségével könnyebben nyerhet saját bérlőt [az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef8f9-108">Get your own tenant by using hello steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="ef8f9-109">Az Azure AD B2C hello panel az hello Azure-portálon létrehozott alkalmazások felügyelni a hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-109">Applications created from hello Azure AD B2C blade in hello Azure portal must be managed from hello same location.</span></span> <span data-ttu-id="ef8f9-110">Ha manuálisan szerkeszti a PowerShell vagy egy másik portálon hello B2C alkalmazások, nem támogatott lesz, és nem működik együtt az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-110">If you edit hello B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="ef8f9-111">A részleteket a hello [hibát jelzett az alkalmazások](#faulted-apps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-111">See details in hello [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-toob2c-settings"></a><span data-ttu-id="ef8f9-112">Keresse meg a tooB2C beállítások</span><span class="sxs-lookup"><span data-stu-id="ef8f9-112">Navigate tooB2C settings</span></span>

<span data-ttu-id="ef8f9-113">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , hello hello B2C bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="ef8f9-113">Log in toohello [Azure portal](https://portal.azure.com/) as hello Global Administrator of hello B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

<span data-ttu-id="ef8f9-114">Válassza ki a következő lépések regisztrál hello alkalmazás típusától függően:</span><span class="sxs-lookup"><span data-stu-id="ef8f9-114">Choose next steps based on hello application type you are registering:</span></span>
