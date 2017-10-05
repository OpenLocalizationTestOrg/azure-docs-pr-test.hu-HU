---
title: "Az alkalmazásproxy alkalmazás PingAccess konfigurálása |} Microsoft Docs"
description: "Ismerje meg, hogyan jogosíthatja alkalmazásproxy fejléc-alapú hitelesítést használó alkalmazások PingAccess segítségével"
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
ms.openlocfilehash: a9da67373465cebbdbecae5c8fb8bd0a0ee3c171
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application-to-use-pingaccess"></a><span data-ttu-id="015f9-103">Az alkalmazásproxy alkalmazás PingAccess konfigurálása</span><span class="sxs-lookup"><span data-stu-id="015f9-103">How to configure an Application Proxy application to use PingAccess</span></span>

<span data-ttu-id="015f9-104">Az együttműködés most a PingAccess jogosíthatja alkalmazásproxy fejléc-alapú hitelesítést használó alkalmazások teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="015f9-104">Our collaboration with PingAccess now allows you to extend the benefits of Application Proxy to applications using header-based authentication.</span></span> <span data-ttu-id="015f9-105">Ha az alkalmazások nem a fejlécek, tekintse meg a [egyszeri bejelentkezés dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) egyéb beállítások leírását.</span><span class="sxs-lookup"><span data-stu-id="015f9-105">If your applications do not use headers, see our [Single Sign-On documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) for details on other options.</span></span>

## <a name="overview-of-steps-and-recommended-documents"></a><span data-ttu-id="015f9-106">A lépéseket és a javasolt dokumentumok áttekintése</span><span class="sxs-lookup"><span data-stu-id="015f9-106">Overview of steps and recommended documents</span></span>

<span data-ttu-id="015f9-107">Az alkalmazás konfigurálása PingAccess, négy lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="015f9-107">To configure an application with PingAccess, there are four steps:</span></span>

1.  <span data-ttu-id="015f9-108">Konfigurálja az Application Proxy összekötőket</span><span class="sxs-lookup"><span data-stu-id="015f9-108">Configure Application Proxy Connectors</span></span>

2.  <span data-ttu-id="015f9-109">Az Azure AD Application Proxy alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="015f9-109">Create an Azure AD Application Proxy Application</span></span>

3.  <span data-ttu-id="015f9-110">Töltse le & PingAccess konfigurálása</span><span class="sxs-lookup"><span data-stu-id="015f9-110">Download & Configure PingAccess</span></span>

4.  <span data-ttu-id="015f9-111">Alkalmazások konfigurálása az PingAccess</span><span class="sxs-lookup"><span data-stu-id="015f9-111">Configure Applications in PingAccess</span></span>

<span data-ttu-id="015f9-112">A lépések a részletekért lásd: a [egyszeri bejelentkezés fejlécek dokumentációját](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span><span class="sxs-lookup"><span data-stu-id="015f9-112">For details on each of these steps, see our [Single Sign-On with Headers documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span></span>
