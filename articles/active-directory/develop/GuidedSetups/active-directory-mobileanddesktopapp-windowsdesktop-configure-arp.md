---
title: "Az Azure AD v2 Windows asztali első lépések - Config |} Microsoft Docs"
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
ms.openlocfilehash: 5e83171846517496e221f0a84565cdf7b77514df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="7c9a2-103">Az alkalmazás regisztrációs adatok hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="7c9a2-103">Add the application’s registration information to your app</span></span>
<span data-ttu-id="7c9a2-104">Ebben a lépésben kell hozzáadnia az alkalmazásazonosítót a projekthez.</span><span class="sxs-lookup"><span data-stu-id="7c9a2-104">In this step, you need to add the Application Id to your project.</span></span>

1.  <span data-ttu-id="7c9a2-105">Nyissa meg `App.xaml.cs` , és cserélje le a sort tartalmazó a `ClientId` együtt:</span><span class="sxs-lookup"><span data-stu-id="7c9a2-105">Open `App.xaml.cs` and replace the line containing the `ClientId` with:</span></span>

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a><span data-ttu-id="7c9a2-106">Mi az a következő</span><span class="sxs-lookup"><span data-stu-id="7c9a2-106">What is Next</span></span>

[<span data-ttu-id="7c9a2-107">Tesztelése és érvényesítése</span><span class="sxs-lookup"><span data-stu-id="7c9a2-107">Test and Validate</span></span>](active-directory-mobileanddesktopapp-windowsdesktop-test.md)
