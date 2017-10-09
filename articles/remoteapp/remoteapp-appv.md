---
title: "App-V aaaUsing alkalmazások az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse App-V alkalmazások az Azure Remoteappban."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a>App-V alkalmazások használata az Azure Remoteappban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Egy Azure RemoteApp hibrid gyűjteményt, amelyhez a tartományhoz való csatlakozást is használhatja az App-V alkalmazások.

Mielőtt elkezdené, győződjön meg arról, hogy tooinstall hello App-V 5.1 ügyfél hello legújabb frissítéseit. Toocreate szüksége lesz egy [egyéni lemezkép](remoteapp-create-custom-image.md) hello App-V-ügyfél foglal magában.  

Egyszerű toouse a meglévő App-V infrastruktúra az Azure RemoteApp. Hibrid gyűjtemény központilag telepítik egy Azure virtuális Hálózatot, amely rendelkezik hozzáférési tooyour tartományvezérlő, és hello virtuális gépek csatlakoznak a tartományhoz, kihasználhatja a meglévő App-v infrastruktúra és a központi telepítési módszerek tooeasyily állomás App-V alkalmazás az Azure Remoteappban. Az alábbiakban néhány szempontokat, amelyeket kell ismernie az App-V központi telepítési jelenleg hello típus alapján:

| Konfigurációs beállítások |  | Pozitív | Negatív. |
| --- | --- | --- | --- |
| Kézbesítési módszer |Adatfolyam-továbbítási (igény) |Alkalmazás áll mindig a legújabb és friss hello |Első késleltetés |
| Csatlakoztatva |Leggyorsabb; alkalmazás már szerepel a virtuális gép hello |Túlzott növekedést - vesz fel helyet a kép (127 GB lehet) | |
| Alkalmazás helye tároló |A megosztott tartalom |Az Azure RemoteApp-példány memóriáját alkalmazás fut |Memória és jó kapcsolat toostreaming (fájl) kiszolgáló hello alkalmazást tartalmazó eats |
| Lemez (gyorsítótárazott) |Gyors végrehajtása. Nem függ a rendelkezésre állási tartalomforrás alkalmazás |Túlzott növekedést - vesz fel helyet a kép (127 GB lehet) | |
| Célcsoport-kezelési |Felhasználó |Teljes önálló App-V infrastruktúrájára van szükség | |
| Globális (számítógép) |Előzetes közzététel vagy használatával közzétételi célként kiszolgáló |Van szükség tooupdate a kép: Azure tooupdate hello app (nagy). Foglal helyet a lemezképet. | |

 Az egyéni lemezképet, és a hibrid gyűjtemény létrehozása után az alkalmazás közzétételére, felhasználók hozzárendelése és a meglévő App-V alkalmazások tooany eszközről, bárhonnan kézbesíteni Azure Remoteappban üzemeltetett élvez.

