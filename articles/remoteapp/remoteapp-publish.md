---
title: "aaaPublish egy alkalmazást az Azure Remoteappban |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish alkalmazásokat és erőforrásokat az Azure Remoteappban."
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
ms.openlocfilehash: d7d92187e9ed999ac79554c9bb61f56a8eceeb31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopublish-an-app-in-remoteapp"></a><span data-ttu-id="84fb9-103">Hogyan toopublish RemoteApp alkalmazás</span><span class="sxs-lookup"><span data-stu-id="84fb9-103">How toopublish an app in RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="84fb9-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="84fb9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="84fb9-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="84fb9-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="84fb9-106">A RemoteApp-gyűjtemény létrehozása után kell toopublish hello alkalmazásokhoz és erőforrásokhoz, amelyet az toomake érhető el a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="84fb9-106">After you create your RemoteApp collection, you need toopublish hello apps or resources that you want toomake available for your users.</span></span> <span data-ttu-id="84fb9-107">hello az előfizetéskor megadott sablonrendszerképek csak néhány alkalmazásokkal rendelkeznek alapértelmezett - által közzétett tooshare hello más alkalmazásokat, toopublish kell őket.</span><span class="sxs-lookup"><span data-stu-id="84fb9-107">hello template images provided with your subscription only have a few apps published by default - tooshare hello other apps, you need toopublish them.</span></span>

> [!NOTE]
> <span data-ttu-id="84fb9-108">Kell-e egy alkalmazás tooupdate?</span><span class="sxs-lookup"><span data-stu-id="84fb9-108">Do you need tooupdate an app?</span></span> <span data-ttu-id="84fb9-109">Szüksége lesz túl[frissítés hello kép](remoteapp-update.md) első.</span><span class="sxs-lookup"><span data-stu-id="84fb9-109">You'll need too[update hello image](remoteapp-update.md) first.</span></span>
> 
> 

<span data-ttu-id="84fb9-110">A hello **közzétételi** hello portálon lapra, majd **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="84fb9-110">On hello **Publishing** tab in hello portal, click **Publish**.</span></span> <span data-ttu-id="84fb9-111">A sablon lemezképe származó is hozzáadhat egy alkalmazást vagy **Start** menü, vagy adjon meg hello sablonlemezkép hello elérési toowhere hello alkalmazást telepítette.</span><span class="sxs-lookup"><span data-stu-id="84fb9-111">You can either add an app from your template image's **Start** menu or provide hello path toowhere hello app is installed on hello template image.</span></span> <span data-ttu-id="84fb9-112">Ha tooadd választhat hello **Start** menü hello app toopublish választ hello listáról.</span><span class="sxs-lookup"><span data-stu-id="84fb9-112">If you choose tooadd from hello **Start** menu, choose hello app toopublish from hello list.</span></span> <span data-ttu-id="84fb9-113">Ha úgy dönt, hogy tooprovide hello elérési toohello alkalmazást, adja meg a hello alkalmazás és hello elérési toohello alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="84fb9-113">If you choose tooprovide hello path toohello app, enter a name for hello app and hello path toohello app.</span></span> <span data-ttu-id="84fb9-114">A változókkal hello elérési út – például "% systemdrive %" helyett "c:\".</span><span class="sxs-lookup"><span data-stu-id="84fb9-114">Use variables in hello path - for example, "%systemdrive%" instead of "c:\".</span></span>

> [!NOTE]
> <span data-ttu-id="84fb9-115">Ha azt szeretné, tooadd hello az alkalmazását **Start** menü toohave kell *hozzá az adott alkalmazás toohello **Start** a sablon rendszerképre menü.*</span><span class="sxs-lookup"><span data-stu-id="84fb9-115">If you want tooadd your app from hello **Start** menu, you need toohave *added that app toohello **Start** menu on your template image.*</span></span> <span data-ttu-id="84fb9-116">Ellenkező esetben RemoteApp csak jelenik meg, mi *van* a hello **Start** menüt, és keverendő lesz.</span><span class="sxs-lookup"><span data-stu-id="84fb9-116">Otherwise, RemoteApp will only see what *is* on hello **Start** menu, and you will be confused.</span></span> 
> 
> <span data-ttu-id="84fb9-117">toomake meg arról, hogy az alkalmazás megtalálható hello **Start** menü helyezhető el egy helyi fájl - **.lnk** – hello %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs mappában található.</span><span class="sxs-lookup"><span data-stu-id="84fb9-117">toomake sure your app is in hello **Start** menu, place a shortcut file - **.lnk** - inside hello %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs folder.</span></span>
> 
> <span data-ttu-id="84fb9-118">Ha elfelejtette tooadd hello app toohello **Start** menü hello sablon létrehozásakor válassza ki a tooadd hello elérési toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84fb9-118">If you forgot tooadd hello app toohello **Start** menu when you created hello template, choose tooadd hello path toohello app.</span></span> <span data-ttu-id="84fb9-119">(Vagy az hozza létre újra a sablon lemezképe, de a Ez meglehetősen egy kicsit nagyobb munkahelyi.)</span><span class="sxs-lookup"><span data-stu-id="84fb9-119">(Or recreate your template image, but that's quite a bit more work.)</span></span>
> 
> 

