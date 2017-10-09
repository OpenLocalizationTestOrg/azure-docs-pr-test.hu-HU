---
title: "aaaAzure AD v2 Windows asztali bevezetés - Config |} Microsoft Docs"
description: "A Windows asztali .NET (XAML) alkalmazások hogyan szereznie egy hozzáférési jogkivonatot, és a hívja az API-k védi az Azure Active Directory v2 végpont."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: d96d9eded200824a6f7ee234009dd0bb11b18b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="41ad5-103">Hello alkalmazás regisztrációs információ tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="41ad5-103">Add hello application’s registration information tooyour app</span></span>
<span data-ttu-id="41ad5-104">Ebben a lépésben kell tooadd hello alkalmazásazonosító tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="41ad5-104">In this step, you need tooadd hello Application Id tooyour project.</span></span>

1.  <span data-ttu-id="41ad5-105">Nyissa meg `App.xaml.cs` , és cserélje le a hello tartalmazó hello sor `ClientId` együtt:</span><span class="sxs-lookup"><span data-stu-id="41ad5-105">Open `App.xaml.cs` and replace hello line containing hello `ClientId` with:</span></span>

```csharp
private static string ClientId = "[Enter hello application Id here]";
```

### <a name="what-is-next"></a><span data-ttu-id="41ad5-106">Mi az a következő</span><span class="sxs-lookup"><span data-stu-id="41ad5-106">What is Next</span></span>

[<span data-ttu-id="41ad5-107">Tesztelése és érvényesítése</span><span class="sxs-lookup"><span data-stu-id="41ad5-107">Test and Validate</span></span>](active-directory-mobileanddesktopapp-windowsdesktop-test.md)
