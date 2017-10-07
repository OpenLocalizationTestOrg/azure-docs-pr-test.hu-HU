---
title: "Bővítmények alkalmazás – az Azure AD B2C |} Microsoft Docs"
description: "B2c-bővítmények-alkalmazás hello visszaállítása"
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
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a><span data-ttu-id="86555-103">Az Azure AD B2C: Bővítmények alkalmazás</span><span class="sxs-lookup"><span data-stu-id="86555-103">Azure AD B2C: Extensions app</span></span>

<span data-ttu-id="86555-104">Az Azure AD B2C-címtárban jön létre, amikor egy alkalmazás nevű `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` hello új könyvtárán belül automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="86555-104">When an Azure AD B2C directory is created, an app called `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` is automatically created inside hello new directory.</span></span> <span data-ttu-id="86555-105">Az alkalmazás, a hivatkozott tooas hello **b2c-bővítmények-alkalmazás**, látható *App regisztrációk*.</span><span class="sxs-lookup"><span data-stu-id="86555-105">This app, referred tooas hello **b2c-extensions-app**, is visible in *App registrations*.</span></span> <span data-ttu-id="86555-106">Az Azure AD B2C szolgáltatás toostore szereplő felhasználók és az egyéni attribútumok hello használják.</span><span class="sxs-lookup"><span data-stu-id="86555-106">It is used by hello Azure AD B2C service toostore information about users and custom attributes.</span></span> <span data-ttu-id="86555-107">Ha hello alkalmazást törlik, az Azure AD B2C nem működik megfelelően, és az éles környezetben milyen hatással lesz.</span><span class="sxs-lookup"><span data-stu-id="86555-107">If hello app is deleted, Azure AD B2C will not function correctly and your production environment will be affected.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86555-108">Ne törölje hello b2c-bővítmények-alkalmazás, kivéve, ha azt tervezi, tooimmediately törlése a bérlő.</span><span class="sxs-lookup"><span data-stu-id="86555-108">Do not delete hello b2c-extensions-app unless you are planning tooimmediately delete your tenant.</span></span> <span data-ttu-id="86555-109">Ha több mint 30 napig hello app marad törölt, felhasználói adatok végleg elvesznek.</span><span class="sxs-lookup"><span data-stu-id="86555-109">If hello app remains deleted for more than 30 days, user information will be permanently lost.</span></span>

## <a name="verifying-that-hello-extensions-app-is-present"></a><span data-ttu-id="86555-110">Jelen hello bővítmények alkalmazást ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="86555-110">Verifying that hello extensions app is present</span></span>

<span data-ttu-id="86555-111">jelen, amely a b2c-bővítmények-alkalmazás hello tooverify:</span><span class="sxs-lookup"><span data-stu-id="86555-111">tooverify that hello b2c-extensions-app is present:</span></span>

1. <span data-ttu-id="86555-112">Az Azure AD B2C-bérlő belül kattintson a **további szolgáltatások** hello bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="86555-112">Inside your Azure AD B2C tenant, click on **More services** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="86555-113">Keresse meg, és nyissa meg a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="86555-113">Search for and open **App registrations**.</span></span>
1. <span data-ttu-id="86555-114">Keressen olyan alkalmazás, amelynek kezdődik **b2c-bővítmények-alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="86555-114">Look for an app that begins with **b2c-extensions-app**</span></span>

## <a name="recover-hello-extensions-app"></a><span data-ttu-id="86555-115">Hello bővítmények app helyreállítása</span><span class="sxs-lookup"><span data-stu-id="86555-115">Recover hello extensions app</span></span>

<span data-ttu-id="86555-116">Ha véletlenül törli a b2c-bővítmények-alkalmazás hello, hogy van-e 30 nap toorecover azt.</span><span class="sxs-lookup"><span data-stu-id="86555-116">If you accidentally deleted hello b2c-extensions-app, you have 30 days toorecover it.</span></span> <span data-ttu-id="86555-117">Visszaállíthatja a hello alkalmazásokhoz hello Graph API használatával:</span><span class="sxs-lookup"><span data-stu-id="86555-117">You can restore hello app using hello Graph API:</span></span>

1. <span data-ttu-id="86555-118">Keresse meg a túl[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="86555-118">Browse too[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span>
1. <span data-ttu-id="86555-119">Jelentkezzen be toohello hely hello Azure AD B2C-címtár globális rendszergazdája, amelyet toorestore törölt hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="86555-119">Log in toohello site as a global administrator for hello Azure AD B2C directory that you want toorestore hello deleted app for.</span></span>
1. <span data-ttu-id="86555-120">Egy HTTP GET hello URL ki `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` az api-version 1.6-os =.</span><span class="sxs-lookup"><span data-stu-id="86555-120">Issue an HTTP GET against hello URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` with api-version=1.6.</span></span> <span data-ttu-id="86555-121">Győződjön meg arról, hogy tooreplace `{tenantName}` a bérlő névvel.</span><span class="sxs-lookup"><span data-stu-id="86555-121">Make sure tooreplace `{tenantName}` with your tenant name.</span></span> <span data-ttu-id="86555-122">Ez a művelet felsorolja az összes törölt hello belül az elmúlt 30 napban hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="86555-122">This operation will list all of hello applications that have been deleted within hello past 30 days.</span></span>
1. <span data-ttu-id="86555-123">Hello alkalmazás található hello lista, amelyen hello karaktersorozattal kezdődő "b2c-bővítmény-alkalmazás", és másolja a `objectid` tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="86555-123">Find hello application in hello list where hello name begins with 'b2c-extension-app’ and copy its `objectid` property value.</span></span>
1. <span data-ttu-id="86555-124">Hello URL HTTP POST ki `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span><span class="sxs-lookup"><span data-stu-id="86555-124">Issue an HTTP POST against hello URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span></span> <span data-ttu-id="86555-125">Cserélje le a hello `{OBJECTID}` hello hello URL-cím része `objectid` hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="86555-125">Replace hello `{OBJECTID}` portion of hello URL with hello `objectid` from hello previous step.</span></span> 

<span data-ttu-id="86555-126">Most tudnia kell túl[lásd: a visszaállított hello app](#verifying-that-the-extensions-app-is-present) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="86555-126">You should now be able too[see hello restored app](#verifying-that-the-extensions-app-is-present) in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="86555-127">Az alkalmazás csak akkor állítható vissza, ha törölték belül hello utolsó 30 nap.</span><span class="sxs-lookup"><span data-stu-id="86555-127">An application can only be restored if it has been deleted within hello last 30 days.</span></span> <span data-ttu-id="86555-128">Ha több mint 30 napig lett, adat végleg elvesznek.</span><span class="sxs-lookup"><span data-stu-id="86555-128">If it has been more than 30 days, data will be permanently lost.</span></span> <span data-ttu-id="86555-129">Ha további segítségre van szüksége a fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="86555-129">For more assistance, file a support ticket.</span></span>
