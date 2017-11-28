---
title: "A felhasználó hozzáférést egy alkalmazáshoz eltávolítása |} Microsoft Docs"
description: "Megtudhatja, hogyan távolítsa el a felhasználó hozzáférést egy alkalmazáshoz"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 497429e7bf62f7e1d67ea429d6b858725f843688
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-remove-a-users-access-to-an-application"></a><span data-ttu-id="f92a3-103">A felhasználó hozzáférést egy alkalmazáshoz eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f92a3-103">How to remove a user's access to an application</span></span>

<span data-ttu-id="f92a3-104">Ez a cikk segítenek megérteni, hogyan távolíthatja el egy felhasználó hozzáférést egy alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f92a3-104">This article help you to understand how to remove a user's access to an application.</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="f92a3-105">Egy alkalmazás egy adott felhasználó vagy csoport-hozzárendelés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f92a3-105">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="f92a3-106">Egy felhasználó vagy csoport-hozzárendelés alkalmazáshoz való eltávolításához kövesse a lépéseket a [egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="f92a3-106">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

<span data-ttu-id="f92a3-107">. ## szeretném tiltani minden hozzáférést egy alkalmazáshoz minden felhasználó</span><span class="sxs-lookup"><span data-stu-id="f92a3-107">.## I want to disable all access to an application for every user</span></span>

<span data-ttu-id="f92a3-108">Minden felhasználói bejelentkezések alkalmazáshoz való letiltásához kövesse a lépéseket a [tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="f92a3-108">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="f92a3-109">Az alkalmazás teljesen törlése</span><span class="sxs-lookup"><span data-stu-id="f92a3-109">I want to delete an application entirely</span></span>

<span data-ttu-id="f92a3-110">A **alkalmazás törlése**, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="f92a3-110">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="f92a3-111">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="f92a3-111">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f92a3-112">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="f92a3-112">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f92a3-113">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f92a3-113">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f92a3-114">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="f92a3-114">Click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f92a3-115">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="f92a3-115">Click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="f92a3-116">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="f92a3-116">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f92a3-117">Válassza ki a törölni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f92a3-117">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="f92a3-118">Ha az alkalmazás betölt, kattintson **törlése** ikonra a felső alkalmazás **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="f92a3-118">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="f92a3-119">Szeretném tiltani az összes jövőbeni felhasználói hozzájárulás műveletek bármely alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f92a3-119">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="f92a3-120">Felhasználói hozzájárulás letiltása, az a teljes címtár megakadályozhatja a végfelhasználók számára hozzájárul ahhoz, hogy minden alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f92a3-120">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="f92a3-121">A rendszergazdák továbbra is a felhasználó behalves is hozzájárulás.</span><span class="sxs-lookup"><span data-stu-id="f92a3-121">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="f92a3-122">Tudjon meg többet a kérelem jóváhagyását, és ezért előfordulhat, hogy, vagy előfordulhat, hogy nem szeretne ehhez olvassa el a [ismertetése felhasználói és rendszergazdai hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="f92a3-122">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="f92a3-123">A **tiltsa le a teljes címtár minden jövőbeni felhasználói hozzájárulás műveletei**, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="f92a3-123">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="f92a3-124">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="f92a3-124">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f92a3-125">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="f92a3-125">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f92a3-126">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f92a3-126">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f92a3-127">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="f92a3-127">Click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="f92a3-128">Kattintson a **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="f92a3-128">Click **User settings**.</span></span>

6.  <span data-ttu-id="f92a3-129">Tiltsa le az összes jövőbeni felhasználói hozzájárulás műveletek úgy, hogy a **felhasználók is engedélyezi, hogy az alkalmazások hozzáférjenek az adataikhoz** kapcsolót **nem** , és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f92a3-129">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>


# <a name="next-steps"></a><span data-ttu-id="f92a3-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f92a3-130">Next steps</span></span>
[<span data-ttu-id="f92a3-131">Az alkalmazásokhoz való hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="f92a3-131">Managing access to apps</span></span>](active-directory-managing-access-to-apps.md)
