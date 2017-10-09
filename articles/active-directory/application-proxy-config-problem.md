---
title: "Az alkalmazásproxy-alkalmazás létrehozása aaaProblem |} Microsoft Docs"
description: "Hogyan tootroubleshoot problémák létrehozása alkalmazásproxy alkalmazások hello Azure AD felügyeleti portál"
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
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="af080-103">A probléma az alkalmazásproxy-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="af080-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="af080-104">Az alábbiakban néhány gyakori problémákat hello személyek arcfelismerési egy új application proxy alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="af080-104">Below are some of hello common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="af080-105">Ajánlott dokumentumok</span><span class="sxs-lookup"><span data-stu-id="af080-105">Recommended documents</span></span> 

<span data-ttu-id="af080-106">További információ az alkalmazásproxy alkalmazások hello felügyeleti portálon keresztül létrehozására toolearn lásd: [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="af080-106">toolearn more about creating an Application Proxy application through hello Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="af080-107">Ha hello dokumentum lépéseit követi, és kihozhatják hello alkalmazás létrehozásakor hiba lépett fel, tekintse meg a hello hiba részletes információkat, és hogyan toofix hello alkalmazás javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="af080-107">If you are following hello steps in that document and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="af080-108">A legtöbb hiba üzenetekben javasolt javítás.</span><span class="sxs-lookup"><span data-stu-id="af080-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-toocheck"></a><span data-ttu-id="af080-109">Konkrét dolgot toocheck</span><span class="sxs-lookup"><span data-stu-id="af080-109">Specific things toocheck</span></span>

<span data-ttu-id="af080-110">tooavoid gyakori hibákat, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="af080-110">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="af080-111">Az engedély toocreate proxyval alkalmazás rendszergazda</span><span class="sxs-lookup"><span data-stu-id="af080-111">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="af080-112">hello belső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="af080-112">hello internal URL is unique</span></span>

-   <span data-ttu-id="af080-113">hello külső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="af080-113">hello external URL is unique</span></span>

-   <span data-ttu-id="af080-114">hello http vagy https URL-címek kezdődik, és a szöveg végén a "/"</span><span class="sxs-lookup"><span data-stu-id="af080-114">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="af080-115">hello URL-cím a tartomány neve és IP-cím nem kell lennie.</span><span class="sxs-lookup"><span data-stu-id="af080-115">hello URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="af080-116">hello hibaüzenet jelenik meg hello jobb felső sarokban található hello alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="af080-116">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="af080-117">Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="af080-117">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="af080-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af080-119">Next steps</span></span>
[<span data-ttu-id="af080-120">Alkalmazásproxy engedélyezése az Azure-portálon hello</span><span class="sxs-lookup"><span data-stu-id="af080-120">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
