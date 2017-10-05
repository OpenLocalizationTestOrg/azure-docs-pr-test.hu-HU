---
title: "Bővítmények alkalmazás – az Azure AD B2C |} Microsoft Docs"
description: "A b2c-bővítmények-alkalmazás visszaállítása"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: 17500b572a0e92c1c233c6967840a5b6d96e21cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-extensions-app"></a><span data-ttu-id="ea78e-103">Az Azure AD B2C: Bővítmények alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ea78e-103">Azure AD B2C: Extensions app</span></span>

<span data-ttu-id="ea78e-104">Az Azure AD B2C-címtárban jön létre, amikor egy alkalmazás nevű `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` automatikusan létrejön az új könyvtárán belül.</span><span class="sxs-lookup"><span data-stu-id="ea78e-104">When an Azure AD B2C directory is created, an app called `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` is automatically created inside the new directory.</span></span> <span data-ttu-id="ea78e-105">Az alkalmazáshoz, a továbbiakban a **b2c-bővítmények-alkalmazás**, látható *App regisztrációk*.</span><span class="sxs-lookup"><span data-stu-id="ea78e-105">This app, referred to as the **b2c-extensions-app**, is visible in *App registrations*.</span></span> <span data-ttu-id="ea78e-106">Segítségével az Azure AD B2C szolgáltatás felhasználók és az egyéni attribútumok adatainak tárolására.</span><span class="sxs-lookup"><span data-stu-id="ea78e-106">It is used by the Azure AD B2C service to store information about users and custom attributes.</span></span> <span data-ttu-id="ea78e-107">Ha az alkalmazást törlik, az Azure AD B2C nem működik megfelelően, és az éles környezetben milyen hatással lesz.</span><span class="sxs-lookup"><span data-stu-id="ea78e-107">If the app is deleted, Azure AD B2C will not function correctly and your production environment will be affected.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea78e-108">Ne törölje a b2c-bővítmények-alkalmazás, kivéve, ha azt tervezi, hogy a bérlő azonnal törli.</span><span class="sxs-lookup"><span data-stu-id="ea78e-108">Do not delete the b2c-extensions-app unless you are planning to immediately delete your tenant.</span></span> <span data-ttu-id="ea78e-109">Ha az alkalmazás több mint 30 napig törölt marad, felhasználói adatok végleg elvesznek.</span><span class="sxs-lookup"><span data-stu-id="ea78e-109">If the app remains deleted for more than 30 days, user information will be permanently lost.</span></span>

## <a name="verifying-that-the-extensions-app-is-present"></a><span data-ttu-id="ea78e-110">Ellenőrzi, hogy jelen-e a bővítmények alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ea78e-110">Verifying that the extensions app is present</span></span>

<span data-ttu-id="ea78e-111">Annak ellenőrzéséhez, hogy a b2c-bővítmények-alkalmazás jelen:</span><span class="sxs-lookup"><span data-stu-id="ea78e-111">To verify that the b2c-extensions-app is present:</span></span>

1. <span data-ttu-id="ea78e-112">Az Azure AD B2C-bérlő belül kattintson a **további szolgáltatások** a bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="ea78e-112">Inside your Azure AD B2C tenant, click on **More services** in the left-hand navigation menu.</span></span>
1. <span data-ttu-id="ea78e-113">Keresse meg, és nyissa meg a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="ea78e-113">Search for and open **App registrations**.</span></span>
1. <span data-ttu-id="ea78e-114">Keressen olyan alkalmazás, amelynek kezdődik **b2c-bővítmények-alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="ea78e-114">Look for an app that begins with **b2c-extensions-app**</span></span>

## <a name="recover-the-extensions-app"></a><span data-ttu-id="ea78e-115">A bővítmények app helyreállítása</span><span class="sxs-lookup"><span data-stu-id="ea78e-115">Recover the extensions app</span></span>

<span data-ttu-id="ea78e-116">Ha véletlenül törli a b2c-bővítmények-alkalmazás, végezze el a helyreállítást 30 napja van.</span><span class="sxs-lookup"><span data-stu-id="ea78e-116">If you accidentally deleted the b2c-extensions-app, you have 30 days to recover it.</span></span> <span data-ttu-id="ea78e-117">Visszaállíthatja az alkalmazást, a Graph API-val:</span><span class="sxs-lookup"><span data-stu-id="ea78e-117">You can restore the app using the Graph API:</span></span>

1. <span data-ttu-id="ea78e-118">Keresse meg a [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="ea78e-118">Browse to [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span>
1. <span data-ttu-id="ea78e-119">Jelentkezzen be a webhely, amelyet szeretne visszaállítani a törölt alkalmazást az Azure AD B2C könyvtár globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ea78e-119">Log in to the site as a global administrator for the Azure AD B2C directory that you want to restore the deleted app for.</span></span>
1. <span data-ttu-id="ea78e-120">Az URL egy HTTP GET ki `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` az api-version 1.6-os =.</span><span class="sxs-lookup"><span data-stu-id="ea78e-120">Issue an HTTP GET against the URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` with api-version=1.6.</span></span> <span data-ttu-id="ea78e-121">Győződjön meg arról, hogy `{tenantName}` a bérlő névvel.</span><span class="sxs-lookup"><span data-stu-id="ea78e-121">Make sure to replace `{tenantName}` with your tenant name.</span></span> <span data-ttu-id="ea78e-122">Ez a művelet felsorolja összes alkalmazást, amely az elmúlt 30 napban törölve lett.</span><span class="sxs-lookup"><span data-stu-id="ea78e-122">This operation will list all of the applications that have been deleted within the past 30 days.</span></span>
1. <span data-ttu-id="ea78e-123">Alkalmazás található a listában, ahol a karaktersorozattal kezdődő "b2c-bővítmény-alkalmazás", és másolja a `objectid` tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="ea78e-123">Find the application in the list where the name begins with 'b2c-extension-app’ and copy its `objectid` property value.</span></span>
1. <span data-ttu-id="ea78e-124">Az URL HTTP POST ki `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span><span class="sxs-lookup"><span data-stu-id="ea78e-124">Issue an HTTP POST against the URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span></span> <span data-ttu-id="ea78e-125">Cserélje le a `{OBJECTID}` URL-CÍMÉT a részét a `objectid` az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="ea78e-125">Replace the `{OBJECTID}` portion of the URL with the `objectid` from the previous step.</span></span> 

<span data-ttu-id="ea78e-126">Meg kell tudni [lásd: a visszaállított app](#verifying-that-the-extensions-app-is-present) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ea78e-126">You should now be able to [see the restored app](#verifying-that-the-extensions-app-is-present) in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="ea78e-127">Az alkalmazás csak akkor állítható vissza, ha törölve lett az utolsó 30 napban.</span><span class="sxs-lookup"><span data-stu-id="ea78e-127">An application can only be restored if it has been deleted within the last 30 days.</span></span> <span data-ttu-id="ea78e-128">Ha több mint 30 napig lett, adat végleg elvesznek.</span><span class="sxs-lookup"><span data-stu-id="ea78e-128">If it has been more than 30 days, data will be permanently lost.</span></span> <span data-ttu-id="ea78e-129">Ha további segítségre van szüksége a fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="ea78e-129">For more assistance, file a support ticket.</span></span>