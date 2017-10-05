---
title: "Az Azure felhőalapú rendszerhéj (előzetes verzió) ablakban |} Microsoft Docs"
description: "Útmutató az Azure felhőalapú rendszerhéj ablakát."
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
ms.openlocfilehash: a47961dfdaf178a6b793bd68105d9792a9275bb3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-azure-cloud-shell-window"></a><span data-ttu-id="8ed81-103">Az Azure felhőalapú rendszerhéj ablakát használatával</span><span class="sxs-lookup"><span data-stu-id="8ed81-103">Using the Azure Cloud Shell window</span></span>

<span data-ttu-id="8ed81-104">Ez a dokumentum ismerteti, hogyan a felhő rendszerhéj ablakát.</span><span class="sxs-lookup"><span data-stu-id="8ed81-104">This document explains how to use the Cloud Shell window.</span></span>

## <a name="concurrent-sessions"></a><span data-ttu-id="8ed81-105">Egyidejű munkamenetek</span><span class="sxs-lookup"><span data-stu-id="8ed81-105">Concurrent sessions</span></span>
<span data-ttu-id="8ed81-106">Felhő rendszerhéj lehetővé teszi több egyidejű munkamenetek különböző böngészőlapokon távfelügyeletét minden munkamenet nem létezik-e külön Bash folyamatban.</span><span class="sxs-lookup"><span data-stu-id="8ed81-106">Cloud Shell enables multiple concurrent sessions across browser tabs by allowing each session to exist as a separate Bash process.</span></span>
<span data-ttu-id="8ed81-107">Ha most kilép egy munkamenet, ügyeljen arra, hogy minden munkamenet ablak lépni, minden folyamat függetlenül fut, de ugyanazon a számítógépen futnak.</span><span class="sxs-lookup"><span data-stu-id="8ed81-107">If exiting a session, be sure to exit from each session window as each process runs independently although they run on the same machine.</span></span>

## <a name="restart-cloud-shell"></a><span data-ttu-id="8ed81-108">Indítsa újra a felhő rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="8ed81-108">Restart Cloud Shell</span></span>
![](media/recycle.png)
> [!WARNING]
> <span data-ttu-id="8ed81-109">Felhő rendszerhéj újraindítása alaphelyzetbe állítja a gép állapotát, és semmilyen fájl nem őrződik meg a fájl megosztási el fog veszni.</span><span class="sxs-lookup"><span data-stu-id="8ed81-109">Restarting Cloud Shell will reset machine state and any files not persisted by your file share will be lost.</span></span>

* <span data-ttu-id="8ed81-110">Kattintson az újraindítás ikonra egy új felhőalapú rendszerhéj környezetben fogadni az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="8ed81-110">Click the restart icon on the toolbar to receive a new Cloud Shell environment.</span></span>

## <a name="minimize--maximize-cloud-shell-window"></a><span data-ttu-id="8ed81-111">Kis méret & felhő rendszerhéj ablakának teljes méretűre állítása</span><span class="sxs-lookup"><span data-stu-id="8ed81-111">Minimize & maximize Cloud Shell window</span></span>
![](media/minmax.png)
* <span data-ttu-id="8ed81-112">Kattintson a kis méret ikonra a felső sarkában az ablak elrejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="8ed81-112">Click the minimize icon on the top right of the window to hide it.</span></span> <span data-ttu-id="8ed81-113">Kattintson a felhő rendszerhéj ismét az ikonra kattintva felfedése.</span><span class="sxs-lookup"><span data-stu-id="8ed81-113">Click the Cloud Shell icon again to unhide.</span></span>
* <span data-ttu-id="8ed81-114">Kattintson a teljes méret ikonra beállítása ablakban maximális magasságát.</span><span class="sxs-lookup"><span data-stu-id="8ed81-114">Click the maximize icon to set window to max height.</span></span> <span data-ttu-id="8ed81-115">Ablak visszaállítása eredeti méretének, kattintson a visszaállítás.</span><span class="sxs-lookup"><span data-stu-id="8ed81-115">To restore window to previous size, click restore.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="8ed81-116">Másolás és beillesztés</span><span class="sxs-lookup"><span data-stu-id="8ed81-116">Copy and paste</span></span>
* <span data-ttu-id="8ed81-117">Windows: `Ctrl-insert` másolása és `Shift-insert` beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="8ed81-117">Windows: `Ctrl-insert` to copy and `Shift-insert` to paste.</span></span> <span data-ttu-id="8ed81-118">Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="8ed81-118">Right-click dropdown can also enable copy/paste.</span></span>
  * <span data-ttu-id="8ed81-119">A FireFox vagy IE nem támogatja a vágólapra engedélyek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="8ed81-119">FireFox/IE may not support clipboard permissions properly.</span></span>
* <span data-ttu-id="8ed81-120">Mac OS: `Cmd-c` másolása és `Cmd-v` beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="8ed81-120">Mac OS: `Cmd-c` to copy and `Cmd-v` to paste.</span></span> <span data-ttu-id="8ed81-121">Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="8ed81-121">Right-click dropdown can also enable copy/paste.</span></span>

## <a name="resize-cloud-shell-window"></a><span data-ttu-id="8ed81-122">Felhő rendszerhéj ablakát átméretezése</span><span class="sxs-lookup"><span data-stu-id="8ed81-122">Resize Cloud Shell window</span></span>
* <span data-ttu-id="8ed81-123">Kattintással és húzással a felső szegélyéhez, hogy az eszköztár felfelé vagy lefelé méretezze át a felhő rendszerhéj ablakát.</span><span class="sxs-lookup"><span data-stu-id="8ed81-123">Click and drag the top edge of the toolbar up or down to resize the Cloud Shell window.</span></span>

## <a name="scrolling-text-display"></a><span data-ttu-id="8ed81-124">A görgethető szöveg megjelenítése</span><span class="sxs-lookup"><span data-stu-id="8ed81-124">Scrolling text display</span></span>
* <span data-ttu-id="8ed81-125">Görgessen az egérrel és touchpad terminál szöveg áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="8ed81-125">Scroll with your mouse or touchpad to move terminal text.</span></span>

## <a name="exit-command"></a><span data-ttu-id="8ed81-126">Kilépés paranccsal</span><span class="sxs-lookup"><span data-stu-id="8ed81-126">Exit command</span></span>
<span data-ttu-id="8ed81-127">Futó `exit` megszakítja az aktív munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="8ed81-127">Running `exit` terminates the active session.</span></span> <span data-ttu-id="8ed81-128">Alapértelmezés szerint ez a viselkedés beavatkozás nélkül 20 perc után következik be.</span><span class="sxs-lookup"><span data-stu-id="8ed81-128">This behavior occurs by default after 20 minutes without interaction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ed81-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ed81-129">Next steps</span></span>
[<span data-ttu-id="8ed81-130">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="8ed81-130">Cloud Shell Quickstart</span></span>](quickstart.md)
