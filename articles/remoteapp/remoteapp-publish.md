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
# <a name="how-to-publish-an-app-in-remoteapp"></a>Hogyan alkalmazás közzététele a Remoteappben
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

A RemoteApp-gyűjtemény létrehozása után, vagy közzé kell tenni az alkalmazások a felhasználók az elérhetővé tenni kívánt erőforrások. Az előfizetéshez mellékelt sablon rendszerképek csak néhány alapértelmezett - megosztása az alkalmazások által közzétett alkalmazásokat, meg kell közzétesszük őket.

> [!NOTE]
> Szüksége frissítheti az alkalmazásokat? Kell [a lemezképek](remoteapp-update.md) első.
> 
> 

Az a **közzétételi** a portálon lapra, majd **közzététel**. A sablon lemezképe származó is hozzáadhat egy alkalmazást vagy **Start** menüben, vagy adjon meg az elérési útját, ahol az alkalmazás telepítve van a sablon lemezképe. Ha úgy dönt, hogy adja hozzá a **Start** menüben válassza ki az alkalmazás közzététele a listából. Ha arra, hogy az alkalmazás elérési útja, írja be egy nevet az alkalmazás és az elérési út az alkalmazás. Ahelyett, hogy az elérési út – például "% systemdrive %" változókkal "c:\".

> [!NOTE]
> Ha hozzá szeretne adni az alkalmazását a **Start** menü kell rendelkeznie *adott alkalmazást felvette a **Start** a sablon rendszerképre menü.* Ellenkező esetben RemoteApp csak jelenik meg, mi *van* a a **Start** menüt, és keverendő lesz. 
> 
> Győződjön meg arról, hogy az alkalmazás megtalálható a **Start** menü helyezhető el egy helyi fájl - **.lnk** – a %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs mappában található.
> 
> Ha az alkalmazás hozzáadása elfelejtette a **Start** a sablon létrehozásakor menüben válassza az alkalmazásnak az elérési út hozzáadása. (Vagy az hozza létre újra a sablon lemezképe, de a Ez meglehetősen egy kicsit nagyobb munkahelyi.)
> 
> 

