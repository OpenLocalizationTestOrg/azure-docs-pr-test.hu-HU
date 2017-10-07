---
title: "aaaAzure RemoteApp gyakorlati tanácsok |} Microsoft Docs"
description: "Ajánlott eljárások az Azure RemoteApp konfigurálásához és használatához."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Ajánlott eljárások az Azure RemoteApp konfigurálásához és használatához
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) részleteiről.
> 
> 

hello a következő információ segítségével konfigurálhatja és használhatja megtartják az Azure RemoteApp.

## <a name="connectivity"></a>Kapcsolatok
* Mindig legyen hello ügyfél letöltéséhez. Használatával a régebbi ügyfelek kapcsolódási problémák és egyéb csökkent tapasztalatokat eredményezheti. Az eszköz az alkalmazás automatikus frissítések engedélyezése biztosítja, hogy hello legújabb ügyfél telepítését mindig a.
* Mindig használjon hello stabil és a megbízható internet kapcsolat elérhető tooyou.  
* Csak támogatott proxyn kapcsolat optimális teljesítmény érdekében.  hello SOCKS proxy nem támogatott.

## <a name="applications"></a>Alkalmazások
* Mentse és zárja be a RemoteApp-alkalmazásokat, ha hello alkalmazással végzett. Nem a hello alkalmazás bezárása adatvesztést eredményezhet.
* Ellenőrizze az egyéni alkalmazások az Azure RemoteApp használatához. Ez magában foglalja, több munkamenet platformon működik, és szükségtelen erőforrást nem például memória és CPU, előfordulhat, hogy egy másik felhasználója hello ki is szoríthatják ugyanahhoz a gyűjteményhez. Információkért töltse le, és tekintse át a hello [alkalmazás kompatibilitási gyakorlati tanácsok a távoli asztali szolgáltatások](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfigurálása és kezelése
* Tartsa meg a sablon rendszerképek mentése toodate, szoftverfrissítések és más kritikus javításokat telepítése, igény szerint. Ez biztosítja, hogy mivel az Azure RemoteApp automatikusan-méretezik toomeet a kapacitás, minden példány telepítve.  
* Győződjön meg arról, hogy az Active Directory összevonási szolgáltatások (AD FS) központi telepítés biztonságos és megbízható. Ellenkező esetben ügyfél hitelesítése sikertelen lehet, az Azure RemoteApp letiltásáról.
* Sablon rendszerképek konfigurálhatja a telepített alkalmazások, szerepkörök vagy szolgáltatások úgy, hogy-e az állapot nélküli. Ezek nem igazolható a virtuális gépek hello állandó állapotának RemoteApp szolgáltatás minden példányát.
  * Összes felhasználói adatot tárolja felhasználói profilok vagy egyéb tárolási helyek külső toohello szolgáltatás, például a helyi fájl megosztásokat vagy a onedrive vállalati verzió.
  * Megosztott adatok tárolása a tárolási helyek külső toohello szolgáltatás, például a helyi fájl megosztásokat vagy a onedrive-on.
  * Bármely rendszerre kiterjedő beállítások hello sablonlemezkép helyett egy szolgáltatás egyes virtuális gépeken.
  * Tiltsa le a közzétett alkalmazások automatikusan a szoftverfrissítéseket - helyette alkalmazza őket manuálisan toohello sablon rendszerképet, és tesztelését hello sablonból központi telepítése előtt.

