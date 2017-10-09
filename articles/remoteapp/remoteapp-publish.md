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
# <a name="how-toopublish-an-app-in-remoteapp"></a>Hogyan toopublish RemoteApp alkalmazás
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

A RemoteApp-gyűjtemény létrehozása után kell toopublish hello alkalmazásokhoz és erőforrásokhoz, amelyet az toomake érhető el a felhasználók számára. hello az előfizetéskor megadott sablonrendszerképek csak néhány alkalmazásokkal rendelkeznek alapértelmezett - által közzétett tooshare hello más alkalmazásokat, toopublish kell őket.

> [!NOTE]
> Kell-e egy alkalmazás tooupdate? Szüksége lesz túl[frissítés hello kép](remoteapp-update.md) első.
> 
> 

A hello **közzétételi** hello portálon lapra, majd **közzététel**. A sablon lemezképe származó is hozzáadhat egy alkalmazást vagy **Start** menü, vagy adjon meg hello sablonlemezkép hello elérési toowhere hello alkalmazást telepítette. Ha tooadd választhat hello **Start** menü hello app toopublish választ hello listáról. Ha úgy dönt, hogy tooprovide hello elérési toohello alkalmazást, adja meg a hello alkalmazás és hello elérési toohello alkalmazás nevét. A változókkal hello elérési út – például "% systemdrive %" helyett "c:\".

> [!NOTE]
> Ha azt szeretné, tooadd hello az alkalmazását **Start** menü toohave kell *hozzá az adott alkalmazás toohello **Start** a sablon rendszerképre menü.* Ellenkező esetben RemoteApp csak jelenik meg, mi *van* a hello **Start** menüt, és keverendő lesz. 
> 
> toomake meg arról, hogy az alkalmazás megtalálható hello **Start** menü helyezhető el egy helyi fájl - **.lnk** – hello %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs mappában található.
> 
> Ha elfelejtette tooadd hello app toohello **Start** menü hello sablon létrehozásakor válassza ki a tooadd hello elérési toohello alkalmazást. (Vagy az hozza létre újra a sablon lemezképe, de a Ez meglehetősen egy kicsit nagyobb munkahelyi.)
> 
> 

