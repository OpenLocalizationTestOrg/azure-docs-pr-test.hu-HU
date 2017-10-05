---
title: "A probléma az alkalmazásproxy-alkalmazás létrehozása |} Microsoft Docs"
description: "Alkalmazásproxy-alkalmazások létrehozása az Azure AD felügyeleti portál kapcsolatos problémák elhárítása"
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
ms.openlocfilehash: fe56f56162ba7186f1b660a5b44fcef38f1dee9d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="23fb4-103">A probléma az alkalmazásproxy-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="23fb4-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="23fb4-104">Az alábbiakban néhány gyakori problémákat személyek arcfelismerési egy új application proxy alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="23fb4-104">Below are some of the common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="23fb4-105">Ajánlott dokumentumok</span><span class="sxs-lookup"><span data-stu-id="23fb4-105">Recommended documents</span></span> 

<span data-ttu-id="23fb4-106">A felügyeleti portálon keresztül alkalmazásproxy alkalmazás létrehozásával kapcsolatos további tudnivalókért lásd: [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="23fb4-106">To learn more about creating an Application Proxy application through the Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="23fb4-107">Ha a dokumentum lépéseit követi, és az alkalmazás létrehozásakor hiba lépett fel kap, tekintse meg információt a hiba részletes adatait, és javaslatokat arról, hogyan javítsa ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="23fb4-107">If you are following the steps in that document and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="23fb4-108">A legtöbb hiba üzenetekben javasolt javítás.</span><span class="sxs-lookup"><span data-stu-id="23fb4-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-to-check"></a><span data-ttu-id="23fb4-109">Adott ellenőrizze az alábbiakat</span><span class="sxs-lookup"><span data-stu-id="23fb4-109">Specific things to check</span></span>

<span data-ttu-id="23fb4-110">Gyakori hibák elkerülése érdekében győződjön meg arról:</span><span class="sxs-lookup"><span data-stu-id="23fb4-110">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="23fb4-111">Alkalmazásproxy alkalmazás létrehozása engedéllyel rendelkező rendszergazdáknak</span><span class="sxs-lookup"><span data-stu-id="23fb4-111">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="23fb4-112">A belső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="23fb4-112">The internal URL is unique</span></span>

-   <span data-ttu-id="23fb4-113">A külső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="23fb4-113">The external URL is unique</span></span>

-   <span data-ttu-id="23fb4-114">Az URL-címének http vagy https kezdődnie, és végén a "/"</span><span class="sxs-lookup"><span data-stu-id="23fb4-114">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="23fb4-115">Az URL-cím a tartomány neve és IP-cím nem kell lennie.</span><span class="sxs-lookup"><span data-stu-id="23fb4-115">The URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="23fb4-116">A jobb felső sarokban a hibaüzenet a következő megjelenjen-e az alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="23fb4-116">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="23fb4-117">Igény szerint kiválaszthatja az értesítés ikonra a hibaüzenetekben talál.</span><span class="sxs-lookup"><span data-stu-id="23fb4-117">You can also select the notification icon to see the error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="23fb4-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23fb4-119">Next steps</span></span>
[<span data-ttu-id="23fb4-120">Alkalmazásproxy engedélyezése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="23fb4-120">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
