---
title: "Alkalmazások közzététele az Azure Remoteappban |} Microsoft Docs"
description: "Ismerje meg, hogyan tehet közzé alkalmazásokat és erőforrásokat az Azure Remoteappban."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c7e1a2cd-8e1f-4a33-bd43-8032ec9ac952
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4565fa498dbadd0601004c73bfee5171efe1fad1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-publish-an-app-in-remoteapp"></a><span data-ttu-id="d5b5f-103">Hogyan alkalmazás közzététele a Remoteappben</span><span class="sxs-lookup"><span data-stu-id="d5b5f-103">How to publish an app in RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d5b5f-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d5b5f-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="d5b5f-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d5b5f-106">A RemoteApp-gyűjtemény létrehozása után, vagy közzé kell tenni az alkalmazások a felhasználók az elérhetővé tenni kívánt erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-106">After you create your RemoteApp collection, you need to publish the apps or resources that you want to make available for your users.</span></span> <span data-ttu-id="d5b5f-107">Az előfizetéshez mellékelt sablon rendszerképek csak néhány alapértelmezett - megosztása az alkalmazások által közzétett alkalmazásokat, meg kell közzétesszük őket.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-107">The template images provided with your subscription only have a few apps published by default - to share the other apps, you need to publish them.</span></span>

> [!NOTE]
> <span data-ttu-id="d5b5f-108">Szüksége frissítheti az alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="d5b5f-108">Do you need to update an app?</span></span> <span data-ttu-id="d5b5f-109">Kell [a lemezképek](remoteapp-update.md) első.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-109">You'll need to [update the image](remoteapp-update.md) first.</span></span>
> 
> 

<span data-ttu-id="d5b5f-110">Az a **közzétételi** a portálon lapra, majd **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-110">On the **Publishing** tab in the portal, click **Publish**.</span></span> <span data-ttu-id="d5b5f-111">A sablon lemezképe származó is hozzáadhat egy alkalmazást vagy **Start** menüben, vagy adjon meg az elérési útját, ahol az alkalmazás telepítve van a sablon lemezképe.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-111">You can either add an app from your template image's **Start** menu or provide the path to where the app is installed on the template image.</span></span> <span data-ttu-id="d5b5f-112">Ha úgy dönt, hogy adja hozzá a **Start** menüben válassza ki az alkalmazás közzététele a listából.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-112">If you choose to add from the **Start** menu, choose the app to publish from the list.</span></span> <span data-ttu-id="d5b5f-113">Ha arra, hogy az alkalmazás elérési útja, írja be egy nevet az alkalmazás és az elérési út az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-113">If you choose to provide the path to the app, enter a name for the app and the path to the app.</span></span> <span data-ttu-id="d5b5f-114">Ahelyett, hogy az elérési út – például "% systemdrive %" változókkal "c:\".</span><span class="sxs-lookup"><span data-stu-id="d5b5f-114">Use variables in the path - for example, "%systemdrive%" instead of "c:\".</span></span>

> [!NOTE]
> <span data-ttu-id="d5b5f-115">Ha hozzá szeretne adni az alkalmazását a **Start** menü kell rendelkeznie *adott alkalmazást felvette a **Start** a sablon rendszerképre menü.*</span><span class="sxs-lookup"><span data-stu-id="d5b5f-115">If you want to add your app from the **Start** menu, you need to have *added that app to the **Start** menu on your template image.*</span></span> <span data-ttu-id="d5b5f-116">Ellenkező esetben RemoteApp csak jelenik meg, mi *van* a a **Start** menüt, és keverendő lesz.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-116">Otherwise, RemoteApp will only see what *is* on the **Start** menu, and you will be confused.</span></span> 
> 
> <span data-ttu-id="d5b5f-117">Győződjön meg arról, hogy az alkalmazás megtalálható a **Start** menü helyezhető el egy helyi fájl - **.lnk** – a %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs mappában található.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-117">To make sure your app is in the **Start** menu, place a shortcut file - **.lnk** - inside the %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs folder.</span></span>
> 
> <span data-ttu-id="d5b5f-118">Ha az alkalmazás hozzáadása elfelejtette a **Start** a sablon létrehozásakor menüben válassza az alkalmazásnak az elérési út hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d5b5f-118">If you forgot to add the app to the **Start** menu when you created the template, choose to add the path to the app.</span></span> <span data-ttu-id="d5b5f-119">(Vagy az hozza létre újra a sablon lemezképe, de a Ez meglehetősen egy kicsit nagyobb munkahelyi.)</span><span class="sxs-lookup"><span data-stu-id="d5b5f-119">(Or recreate your template image, but that's quite a bit more work.)</span></span>
> 
> 

