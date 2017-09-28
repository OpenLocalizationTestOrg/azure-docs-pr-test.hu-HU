---
title: "Sipi fájl tesztelése |} Microsoft Docs"
description: "Fájl tesztelése ReadyForTest függőségek ellenőrzése"
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
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a><span data-ttu-id="16155-103">Sipi fájl tesztelése</span><span class="sxs-lookup"><span data-stu-id="16155-103">Sipi test file</span></span>

<span data-ttu-id="16155-104">Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="16155-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="16155-105">Az útmutató befejeztével az alkalmazás regisztrálva lesz és használatra készen áll az Azure B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="16155-105">When you're finished, your application is registered for use in the Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16155-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="16155-106">Prerequisites</span></span>

<span data-ttu-id="16155-107">A felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás létrehozásához először regisztrálnia kell az alkalmazást egy Azure Active Directory B2C-bérlővel.</span><span class="sxs-lookup"><span data-stu-id="16155-107">To build an application that accepts consumer sign-up and sign-in, you first need to register the application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="16155-108">Hozzon létre egy saját bérlőt az [Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md) részben ismertetett lépések segítségével.</span><span class="sxs-lookup"><span data-stu-id="16155-108">Get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="16155-109">Az Azure Portal Azure AD B2C paneljén létrehozott alkalmazásokat ugyanarról a helyről kell kezelnie.</span><span class="sxs-lookup"><span data-stu-id="16155-109">Applications created from the Azure AD B2C blade in the Azure portal must be managed from the same location.</span></span> <span data-ttu-id="16155-110">Ha a B2C-alkalmazásokat a PowerShell vagy egy másik portál használatával szerkeszti, a rendszer nem támogatja többé az alkalmazásokat, és nem fognak működni az Azure AD B2C-vel.</span><span class="sxs-lookup"><span data-stu-id="16155-110">If you edit the B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="16155-111">További részletek a [hibás alkalmazások](#faulted-apps) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="16155-111">See details in the [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-to-b2c-settings"></a><span data-ttu-id="16155-112">Navigálás a B2C beállításaihoz</span><span class="sxs-lookup"><span data-stu-id="16155-112">Navigate to B2C settings</span></span>

<span data-ttu-id="16155-113">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) a B2C-bérlő globális rendszergazdájaként.</span><span class="sxs-lookup"><span data-stu-id="16155-113">Log in to the [Azure portal](https://portal.azure.com/) as the Global Administrator of the B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

<span data-ttu-id="16155-114">Válassza ki a következő lépéseket a regisztrálandó alkalmazás típusától függően:</span><span class="sxs-lookup"><span data-stu-id="16155-114">Choose next steps based on the application type you are registering:</span></span>
