---
title: "aaaAzure RemoteApp hálózati sávszélesség - általános irányelveket |} Microsoft Docs"
description: "Ismerje meg az Azure RemoteApp-gyűjtemények és az alkalmazások néhány alapvető hálózati sávszélesség útmutatást."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d3407eea71e2e8ac524787ee680cfd870c800864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a><span data-ttu-id="4eaf2-103">Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)</span><span class="sxs-lookup"><span data-stu-id="4eaf2-103">Azure RemoteApp network bandwidth - general guidelines (if you can't test your own)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4eaf2-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4eaf2-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4eaf2-106">Ha nem rendelkezik hello idő vagy funkció toorun hello [hálózati sávszélesség tesztek](remoteapp-bandwidthtests.md) az Azure RemoteApp, az alábbiakban néhány viszonylag általános irányelveket, amelyik segíthet a hálózati sávszélesség felhasználónként becslése.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-106">If you do not have hello time or capability toorun hello [network bandwidth tests](remoteapp-bandwidthtests.md) for Azure RemoteApp, here are some fairly generic guidelines that can help you estimate network bandwidth per user.</span></span>

<span data-ttu-id="4eaf2-107">Ha ezek a forgatókönyvek vegyesen, nem ajánlott semmilyen nagyobb, mint (egyenlő) 10 MB/s, hello minimális sávszélesség internetkapcsolattal rendelkező modern alkalmazások, a távoli környezetekben.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-107">If you have a mix of these scenarios, we don't recommend anything less than (or equal to) 10 MB/s as hello MINIMUM network bandwidth for modern Internet-connected apps in a remote environment.</span></span> <span data-ttu-id="4eaf2-108">(Bár a bemutatott, ez nem garantálja a megfelelőbb mint átlagos felhasználói élményt.)</span><span class="sxs-lookup"><span data-stu-id="4eaf2-108">(Although, as discussed, this will not guarantee a better than average user experience.)</span></span>

## <a name="complex-powerpoint-simple-powerpoint"></a><span data-ttu-id="4eaf2-109">Összetett PowerPoint, egyszerű PowerPoint</span><span class="sxs-lookup"><span data-stu-id="4eaf2-109">Complex PowerPoint, simple PowerPoint</span></span>
<span data-ttu-id="4eaf2-110">Az Azure RemoteApp legjobb 100 MB-os hálózati végzi.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-110">Azure RemoteApp does best on 100 MB LAN.</span></span> <span data-ttu-id="4eaf2-111">A hello a 10 MB/s hálózati profil, ha 120 ms fent jitter több mint 5 %, hello felhasználói élményt átlagos jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-111">At hello 10 MB/s network profile, when jitter above 120 ms is more than 5%, hello user will see an average experience.</span></span> <span data-ttu-id="4eaf2-112">1 MB/s hello különböző van szembeszökő egyenetlenség – például előforduló, hello előfordulhat, hogy látni animált átmenetek minden keretek ki van hagyva, mert.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-112">At 1 MB/s hello different is glaring - for example, in a slide show, hello user might not see animated transitions at all because frames are skipped.</span></span>

## <a name="internet-explorer-mixed-pdf-pdf-text"></a><span data-ttu-id="4eaf2-113">Internet Explorer, a PDF-, PDF-, szöveges vegyes</span><span class="sxs-lookup"><span data-stu-id="4eaf2-113">Internet Explorer, mixed PDF, PDF, Text</span></span>
<span data-ttu-id="4eaf2-114">10 MB/s hálózati profil Bezárás tooLAN a legtöbb területét.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-114">10 MB/s network profile is close tooLAN in most aspects.</span></span> <span data-ttu-id="4eaf2-115">1 MB/s egy OK a felhasználói élményt nyújtják, bár előfordulhat, egyes jitter egy felhasználó görgetésekor miközben képek hello képernyőn.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-115">1 MB/s will provide an OK experience, although there may be some jitter when a user scrolls while there are images on hello screen.</span></span>

## <a name="flash-video-youtube"></a><span data-ttu-id="4eaf2-116">Flash videó (YouTube)</span><span class="sxs-lookup"><span data-stu-id="4eaf2-116">Flash video (YouTube)</span></span>
<span data-ttu-id="4eaf2-117">100 MB/s LAN hello legjobb élményt, biztosít, elfogadható (azaz a rendszer hello képkockasebessége tartani, de növeli szolgáltatás) pedig 10 MB/s.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-117">100 MB/s LAN provides hello best experience, while 10 MB/s is acceptable (meaning we keep up with hello frame rate but jitter increases).</span></span> <span data-ttu-id="4eaf2-118">1 MB/s-t a jitter nagyon magas és észrevehető.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-118">At 1 MB/s, jitter is very high and noticeable.</span></span>

## <a name="word-typing-word-remote-input"></a><span data-ttu-id="4eaf2-119">A Word gépelési (Word távoli bemeneti)</span><span class="sxs-lookup"><span data-stu-id="4eaf2-119">Word typing (Word remote input)</span></span>
<span data-ttu-id="4eaf2-120">Ez egy webfarmos alacsony sávszélességű használat.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-120">This is a low-bandwidth usage scenario.</span></span> <span data-ttu-id="4eaf2-121">256 KB/s nyújtunk olyan jó a LAN élményt.</span><span class="sxs-lookup"><span data-stu-id="4eaf2-121">At 256 KB/s we provide as good of an experience as LAN.</span></span>

## <a name="learn-more"></a><span data-ttu-id="4eaf2-122">Részletek</span><span class="sxs-lookup"><span data-stu-id="4eaf2-122">Learn more</span></span>
* [<span data-ttu-id="4eaf2-123">Azure RemoteApp a sávszélesség-használat becslése</span><span class="sxs-lookup"><span data-stu-id="4eaf2-123">Estimate Azure RemoteApp network bandwidth usage</span></span>](remoteapp-bandwidth.md)
* [<span data-ttu-id="4eaf2-124">Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?</span><span class="sxs-lookup"><span data-stu-id="4eaf2-124">Azure RemoteApp - how do network bandwidth and quality of experience work together?</span></span>](remoteapp-bandwidthexperience.md)
* [<span data-ttu-id="4eaf2-125">Az Azure RemoteApp - tseting olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat</span><span class="sxs-lookup"><span data-stu-id="4eaf2-125">Azure RemoteApp - tseting your network bandwidth usage with some common scenarios</span></span>](remoteapp-bandwidthtests.md)

