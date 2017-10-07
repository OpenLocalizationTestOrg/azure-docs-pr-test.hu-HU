---
title: "Azure virtuális gépek a Server Explorer aaaAccessing |} Microsoft Docs"
description: "Hogyan tooview létrehozása és kezelése az Azure virtuális gépek (VM) a Visual Studio Server Explorer áttekintést kaphat."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>A Server Explorer Azure virtuális gépek elérése
A Visual Studio Server Explorer használatával jelenítheti meg információkat az Azure által futtatott virtuális gépek kapcsolatban.

## <a name="accessing-virtual-machines-in-server-explorer"></a>A Server Explorer virtuális gépek elérése
Ha az Azure-ban üzemeltetett virtuális gépeket, a Server Explorer is elérhet. Ön először be kell jelentkeznie az Azure-előfizetés tooview tooyour a mobile services. toosign, nyissa meg a helyi menüje hello hello Azure csomópont a Server Explorer, és válassza a **tooMicrosoft Azure csatlakozás**.

### <a name="tooget-information-about-your-virtual-machines"></a>a virtuális gépek tooget információ
1. A Server Explorer eszközben válassza ki a virtuális gépet, és a Tulajdonságok ablak hello F4 kulcs tooshow válassza.
   
    hello a következő táblázat bemutatja, milyen tulajdonságok érhetőek el, de összes csak olvasható. toochange, használja a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
   
   | Tulajdonság | Leírás |
   | --- | --- |
   | DNS-név |hello URL-cím elé hello internetcím hello virtuális gép. |
   | Környezet |A virtuális gép hello Ez a tulajdonság értéke mindig éles. |
   | Név |hello hello virtuális gép nevét. |
   | Méret |hello virtuális gép, amely tükrözi a rendelkezésre álló szabad memória és lemezterület mennyisége hello hello mérete. További információkért lásd: Útmutató: konfigurálja a virtuális gépek méretét. |
   | status |Értékek: indítása, elindítva, leállítása, leállítva vagy Állapot lekérése során. Ha beolvasása állapot jelenik meg, hello aktuális állapota ismeretlen. hello értékek ehhez a tulajdonsághoz eltérnek a hello használt hello értékek [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885). |
   | Előfizetés-azonosító |hello előfizetési Azonosítóját az Azure-fiókjával. Ezek az információk megjelenítése hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello előfizetés tulajdonságai között egy megtekintésével. |
2. Válasszon egy végpont csomópont, és nézze meg hello **tulajdonságok** ablak.
3. hello következő táblázat ismerteti hello rendelkezésre álló tulajdonságok végpontok, de csak olvasható. a virtuális gép tooadd vagy Szerkesztés hello végpontok használata hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885). 
   
   | Tulajdonság | Leírás |
   | --- | --- |
   | Név |Hello végpont azonosítója. |
   | Magánhálózati Port |hálózati hozzáférés belső tooyour alkalmazás hello port. |
   | Protokoll |hello protokoll, amely a szállítási réteg végpont hello használ, TCP vagy UDP. |
   | Nyilvános port |hello nyilvános hozzáférés tooyour alkalmazásához használt port. |

## <a name="next-steps"></a>Következő lépések
További információ az Azure szerepkörök a Visual Studióban, toolearn lásd [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).

