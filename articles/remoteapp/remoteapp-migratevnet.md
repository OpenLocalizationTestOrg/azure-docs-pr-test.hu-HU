---
title: "a RemoteApp virtuális Hálózatot tooan Azure virtuális Hálózatot a aaaHow toomigrate |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate a egy RemoteApp virtuális Hálózatot tooan Azure virtuális hálózat"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a>Hogyan toomigrate egy hibrid gyűjteményt a RemoteApp virtuális Hálózatot tooan Azure virtuális hálózat
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Jó hírünk van! Jelenleg engedélyezett, toodeploy hibrid RemoteApp-gyűjtemények közvetlenül be a meglévő Azure virtuális hálózatokról (Vnetekről) RemoteApp-specifikus Vnetek létrehozása helyett. Ez lehetővé teszi előnyeit hello legújabb VNET funkciók (például ExpressRoute), és adjon a hibrid gyűjtemények közvetlen hálózati hozzáférés tooother Azure-szolgáltatások és virtuális gépek rendszerbe toothat virtuális hálózat.  (Ez segít jobb teljesítmény és egyszerűbb a telepítő mint VNET – VNET konfigurációk).

Tegyük fel, hogy már létrehozott egy hibrid RemoteApp-gyűjtemény nevű *OriginalCollection* RemoteApp VNET neve *RemoteAppVNET*. Az alábbiakban hello lépéseket toomigrate az új Azure VNET neve tooa *AzureVNET*.

1. A hello **hálózatok** hello lapján [kezelési portál](http://manage.windowsazure.com/), hozzon létre egy VNETET nevű *AzureVNET*használatával hello ugyanazon a helyen, a DNS-konfiguráció és a címtér (legalább egy a hello *AzureVNET* alhálózatok), ha Ön használt *RemoteAppVNET*.
2. Konfigurálása *AzureVNET* tooeither gazdagép vagy a hálózati kapcsolat toohello Active Directory központi telepítéssel rendelkezik, amely *OriginalCollection* tartományhoz csatlakozik.
3. A hello **távoli alkalmazások** lapra, hozzon létre egy új RemoteApp-gyűjtemény nevű *új gyűjtemény*. (Használata hello **hozzon létre virtuális hálózaton** beállítás, nem **Gyorslétrehozás**.)
4. Konfigurálása *NewCollection* toobe telepített tooa alhálózatának *AzureVNET*.
5. Konfigurálása *NewCollection* toouse hello ugyanazt a képet, és, ha Ön használt tartományi csatlakozási adatainak *OriginalCollection*.
6. Néhány óra elteltével *NewCollection* fog megjelenni az adatgyűjtési lista egy aktív állapotú.

Mostantól Ha nem kívánja toomigrate gyűjteményből hello eredeti gyűjtemény toohello új felhasználói adatokat, teendőkre ezeket a lépéseket:

1. Törlés *OriginalCollection*.
2. Törlés *RemoteAppVNET*.

És elkészült!

Alternatív megoldásként Ha tájékoztatásra van szüksége toomigrate felhasználói hello eredeti gyűjtemény toohello új gyűjtemény, teendőkre ezeket a lépéseket:

1. Az e-mailek küldése túl[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) az Azure-előfizetése Azonosítóját, hello az eredeti gyűjtemény nevét, és az új gyűjtemény hello nevét, és kérje meg toomigrate a felhasználói adatokat.
2. 2 munkanapon belül hello RemoteApp csapatától áthelyezi hello felhasználói hozzáférési lista és az összes felhasználói dokumentumokat és a felhasználói beállítások hello eredeti gyűjtemény toohello új gyűjtemény.
3. Törlés *OriginalCollection*.
4. Törlés *RemoteAppVNET*.

És mostantól elkészült!

Ha kérdése van, vagy különleges segítségre van szüksége, írjon e-mailt [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).

