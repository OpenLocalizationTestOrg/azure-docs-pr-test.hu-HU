---
title: aaaUsing Outlook az Azure Remoteappban |} Microsoft Docs
description: "Megtudhatja, hogyan tooconfigure és használhatja az Outlookot az Azure RemoteApp |} A Microsoft Azure"
services: remoteapp
documentationcenter: 
author: pavithir
manager: mbaldwin
ms.assetid: cb2a498f-9539-4522-a874-542114926a61
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 119d2629ac47bd8d20d617985a9b488068aa959f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>A Microsoft Outlook használata az Azure RemoteAppban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp támogatja a Microsoft Outlook O365 használatát. További információk az [Office működéséről az Azure RemoteAppban](remoteapp-officesubscription.md). Az Outlook az Azure RemoteAppban való használatához van néhány ajánlott beállítás.

## <a name="cached-mode"></a>Gyorsítótáras üzemmód
A gyorsítótáras üzemmód a javasolt konfiguráció, ha az Outlookot az Azure RemoteAppban használja. Amikor konfigurál egy Outlook 2013 fiók toouse gyorsítótáras Exchange üzemmódot, az Outlook 2013 egy offline adatfájlban (OST-fájl) hello felhasználó számítógépén, hello Offline címjegyzék együtt tárolt hello felhasználó Microsoft Exchange-postaládájának helyi másolatából működik-e (OAB). hello gyorsítótárazott postaláda rendszeresen frissülnek offline Címjegyzék, az Office 365 szolgáltatás hello. Tudjon meg többet az [gyorsítótárazott és az online üzemmód közötti különbségekről hello](https://technet.microsoft.com/library/jj683103.aspx).

hello felhasználó kiválaszthatja a kívánt **gyorsítótáras Exchange üzemmódot** vagy **Online módban** fiók a telepítés során, illetve hello fiók beállításainak módosításával. Egy mód vagy más hello hello Office testreszabási eszköz (OCT) vagy a csoportházirend használatával is telepítheti.  

Olvassa el [a gyorsítótáras üzemmód engedélyezésének lépésenkénti útmutatóját](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Keresés
Az Azure RemoteAppban az Outlookon belüli keresésekre korlátozások vonatkoznak. Az Azure RemoteApp készletbe vont virtuális gépek tooaccommodate felhasználói munkamenetek használja. A keresések indexelése attól függ, hogy hello gép azonosítója, amely különbözik a különböző virtuális gépek. Lehetséges, hogy minden alkalommal, amikor egy felhasználó bejelentkezik az Azure RemoteApp, azok irányított tooa új virtuális Gépet. Ez azt jelenti, ha engedélyezzük a helyi keresést, hello indexelő minden alkalommal fut hello gép azonosítója (ha hello felhasználó eltérő virtuális gépet) változik. Attól függően, hogy hello hello mérete. OST-fájl, hello indexelő sikerült egy hosszú ideig toocomplete igénybe vehet, és elhasználja a más alkalmazásokhoz szükséges erőforrásokat. A keresés ilyenkor nemcsak lassú, de előfordulhat, hogy eredményeket sem ad. Egy Online módban fiókprofil használatával volna elkerüléséhez, de általános teljesítménye tartoznának csökkenni fog, a helyi gyorsítótárba toohello hiánya miatt (lásd fent hello különbségének gyorsítótárazott és az online üzemmód további információ hivatkozás hello). Az indexelt/helyi keresés sajnos nem tiltható le, és az online keresés nem engedélyezhető alapértelmezés szerint az Outlook 2013-ban.

