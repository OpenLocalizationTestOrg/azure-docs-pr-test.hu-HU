---
title: "aaaUsing hello Azure Cloud rendszerhéj (előzetes verzió) ablakban |} Microsoft Docs"
description: "A forgatókönyv hello Azure Cloud rendszerhéj ablakát."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.openlocfilehash: 571db3c8e177799a9e05f38a7cf8d2a4d5f8c8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cloud-shell-window"></a><span data-ttu-id="7b2ec-103">Hello Azure Cloud rendszerhéj ablakát használatával</span><span class="sxs-lookup"><span data-stu-id="7b2ec-103">Using hello Azure Cloud Shell window</span></span>

<span data-ttu-id="7b2ec-104">Ez a dokumentum azt ismerteti, hogyan toouse hello felhő rendszerhéj ablakát.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-104">This document explains how toouse hello Cloud Shell window.</span></span>

## <a name="concurrent-sessions"></a><span data-ttu-id="7b2ec-105">Egyidejű munkamenetek</span><span class="sxs-lookup"><span data-stu-id="7b2ec-105">Concurrent sessions</span></span>
<span data-ttu-id="7b2ec-106">Minden munkamenet tooexist Bash különálló folyamatként tételével a felhő rendszerhéj lehetővé teszi több egyidejű munkamenetek különböző böngészőlapokon.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-106">Cloud Shell enables multiple concurrent sessions across browser tabs by allowing each session tooexist as a separate Bash process.</span></span>
<span data-ttu-id="7b2ec-107">Ha most kilép a munkamenet meg arról, hogy minden munkamenet ablakból tooexit minden folyamat függetlenül fut, bár a számítógépen futnak, hello azonos gépen.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-107">If exiting a session, be sure tooexit from each session window as each process runs independently although they run on hello same machine.</span></span>

## <a name="restart-cloud-shell"></a><span data-ttu-id="7b2ec-108">Indítsa újra a felhő rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="7b2ec-108">Restart Cloud Shell</span></span>
![](media/recycle.png)
> [!WARNING]
> <span data-ttu-id="7b2ec-109">Felhő rendszerhéj újraindítása alaphelyzetbe állítja a gép állapotát, és semmilyen fájl nem őrződik meg a fájl megosztási el fog veszni.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-109">Restarting Cloud Shell will reset machine state and any files not persisted by your file share will be lost.</span></span>

* <span data-ttu-id="7b2ec-110">Kattintson a hello újraindítás ikonjára hello eszköztár tooreceive egy új felhőalapú rendszerhéj-környezetben.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-110">Click hello restart icon on hello toolbar tooreceive a new Cloud Shell environment.</span></span>

## <a name="minimize--maximize-cloud-shell-window"></a><span data-ttu-id="7b2ec-111">Kis méret & felhő rendszerhéj ablakának teljes méretűre állítása</span><span class="sxs-lookup"><span data-stu-id="7b2ec-111">Minimize & maximize Cloud Shell window</span></span>
![](media/minmax.png)
* <span data-ttu-id="7b2ec-112">Hello kattintson hello ikonra minimalizálása érdekében jobb oldalán hello ablak toohide felső azt.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-112">Click hello minimize icon on hello top right of hello window toohide it.</span></span> <span data-ttu-id="7b2ec-113">Kattintson a felhő rendszerhéj ikon hello újra toounhide.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-113">Click hello Cloud Shell icon again toounhide.</span></span>
* <span data-ttu-id="7b2ec-114">Kattintson a hello ikon tooset ablak toomax magassága maximalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-114">Click hello maximize icon tooset window toomax height.</span></span> <span data-ttu-id="7b2ec-115">toorestore ablak tooprevious mérete, kattintson a Visszaállítás gombra.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-115">toorestore window tooprevious size, click restore.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="7b2ec-116">Másolás és beillesztés</span><span class="sxs-lookup"><span data-stu-id="7b2ec-116">Copy and paste</span></span>
* <span data-ttu-id="7b2ec-117">Windows: `Ctrl-insert` toocopy és `Shift-insert` toopaste.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-117">Windows: `Ctrl-insert` toocopy and `Shift-insert` toopaste.</span></span> <span data-ttu-id="7b2ec-118">Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-118">Right-click dropdown can also enable copy/paste.</span></span>
  * <span data-ttu-id="7b2ec-119">A FireFox vagy IE nem támogatja a vágólapra engedélyek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-119">FireFox/IE may not support clipboard permissions properly.</span></span>
* <span data-ttu-id="7b2ec-120">Mac OS: `Cmd-c` toocopy és `Cmd-v` toopaste.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-120">Mac OS: `Cmd-c` toocopy and `Cmd-v` toopaste.</span></span> <span data-ttu-id="7b2ec-121">Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-121">Right-click dropdown can also enable copy/paste.</span></span>

## <a name="resize-cloud-shell-window"></a><span data-ttu-id="7b2ec-122">Felhő rendszerhéj ablakát átméretezése</span><span class="sxs-lookup"><span data-stu-id="7b2ec-122">Resize Cloud Shell window</span></span>
* <span data-ttu-id="7b2ec-123">Kattintással és húzással hello eszköztár felső széle hello felfelé vagy lefelé tooresize hello felhő rendszerhéj ablakát.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-123">Click and drag hello top edge of hello toolbar up or down tooresize hello Cloud Shell window.</span></span>

## <a name="scrolling-text-display"></a><span data-ttu-id="7b2ec-124">A görgethető szöveg megjelenítése</span><span class="sxs-lookup"><span data-stu-id="7b2ec-124">Scrolling text display</span></span>
* <span data-ttu-id="7b2ec-125">Görgessen az egeret vagy touchpad toomove terminál szóra.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-125">Scroll with your mouse or touchpad toomove terminal text.</span></span>

## <a name="exit-command"></a><span data-ttu-id="7b2ec-126">Kilépés paranccsal</span><span class="sxs-lookup"><span data-stu-id="7b2ec-126">Exit command</span></span>
<span data-ttu-id="7b2ec-127">Futó `exit` hello aktív munkamenet leállítása.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-127">Running `exit` terminates hello active session.</span></span> <span data-ttu-id="7b2ec-128">Alapértelmezés szerint ez a viselkedés beavatkozás nélkül 20 perc után következik be.</span><span class="sxs-lookup"><span data-stu-id="7b2ec-128">This behavior occurs by default after 20 minutes without interaction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b2ec-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b2ec-129">Next steps</span></span>
[<span data-ttu-id="7b2ec-130">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="7b2ec-130">Cloud Shell Quickstart</span></span>](quickstart.md)
