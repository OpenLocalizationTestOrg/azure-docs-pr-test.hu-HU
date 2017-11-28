---
title: "aaaAzure AD v2.0 univerzális Windows-alkalmazás |} Microsoft Docs"
description: "Hogyan toobuild egy univerzális Windows-alkalmazás, amely bejelentkezik felhasználók mindkét személyes Microsoft Account és munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: d2c92b65-3c1d-46d1-81c8-88f32f6b2c4b
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.date: 02/20/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 49b26c74fa5a76664c3229256c9bd128563b830c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-universal-app-using-hello-v20-endpoint"></a><span data-ttu-id="4e3a7-103">Bejelentkezési tooa hello v2.0-végponttól használatával univerzális Windows-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4e3a7-103">Add sign-in tooa Windows Universal app using hello v2.0 endpoint</span></span>
  <span data-ttu-id="4e3a7-104">hello gyors üzembe helyezési oktatóanyag univerzális Windows-alkalmazásokhoz nem áll teljesen készen... Vissza hamarosan & keresse meg a elérhető frissítések ellenőrzése @AzureAD a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="4e3a7-104">hello quick-start tutorial for Windows Universal apps isn't quite ready... Check back soon & look for updates from @AzureAD on Twitter.</span></span>

> [!NOTE]
> <span data-ttu-id="4e3a7-105">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="4e3a7-105">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="4e3a7-106">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="4e3a7-106">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

    ## Get security updates for our products

<span data-ttu-id="4e3a7-107">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="4e3a7-107">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

