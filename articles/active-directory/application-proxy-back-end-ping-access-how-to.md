---
title: "Az alkalmazásproxy alkalmazás toouse PingAccess aaaHow tooconfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse PingAccess tooextend hello fejléc-alapú hitelesítést használó alkalmazás Proxy tooapplications előnyei"
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
ms.openlocfilehash: fa4c9747b7bf5a135425be270e4f7eadf95248fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-pingaccess"></a><span data-ttu-id="efa66-103">Hogyan tooconfigure az alkalmazásproxy alkalmazás toouse PingAccess</span><span class="sxs-lookup"><span data-stu-id="efa66-103">How tooconfigure an Application Proxy application toouse PingAccess</span></span>

<span data-ttu-id="efa66-104">A együttműködve PingAccess mostantól lehetővé teszi tooextend hello előnyei alkalmazásproxy tooapplications fejléc-alapú hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="efa66-104">Our collaboration with PingAccess now allows you tooextend hello benefits of Application Proxy tooapplications using header-based authentication.</span></span> <span data-ttu-id="efa66-105">Ha az alkalmazások nem a fejlécek, tekintse meg a [egyszeri bejelentkezés dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) egyéb beállítások leírását.</span><span class="sxs-lookup"><span data-stu-id="efa66-105">If your applications do not use headers, see our [Single Sign-On documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) for details on other options.</span></span>

## <a name="overview-of-steps-and-recommended-documents"></a><span data-ttu-id="efa66-106">A lépéseket és a javasolt dokumentumok áttekintése</span><span class="sxs-lookup"><span data-stu-id="efa66-106">Overview of steps and recommended documents</span></span>

<span data-ttu-id="efa66-107">tooconfigure PingAccess, az alkalmazás négy lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="efa66-107">tooconfigure an application with PingAccess, there are four steps:</span></span>

1.  <span data-ttu-id="efa66-108">Konfigurálja az Application Proxy összekötőket</span><span class="sxs-lookup"><span data-stu-id="efa66-108">Configure Application Proxy Connectors</span></span>

2.  <span data-ttu-id="efa66-109">Az Azure AD Application Proxy alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="efa66-109">Create an Azure AD Application Proxy Application</span></span>

3.  <span data-ttu-id="efa66-110">Töltse le & PingAccess konfigurálása</span><span class="sxs-lookup"><span data-stu-id="efa66-110">Download & Configure PingAccess</span></span>

4.  <span data-ttu-id="efa66-111">Alkalmazások konfigurálása az PingAccess</span><span class="sxs-lookup"><span data-stu-id="efa66-111">Configure Applications in PingAccess</span></span>

<span data-ttu-id="efa66-112">A lépések a részletekért lásd: a [egyszeri bejelentkezés fejlécek dokumentációját](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span><span class="sxs-lookup"><span data-stu-id="efa66-112">For details on each of these steps, see our [Single Sign-On with Headers documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span></span>
