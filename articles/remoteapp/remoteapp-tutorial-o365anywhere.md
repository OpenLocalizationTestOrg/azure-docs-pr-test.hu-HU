---
title: "aaaGet hello azonos Office 365-élmény bármely Azure RemoteApp eszközön |} Microsoft Docs"
description: "Megtudhatja, hogyan tooshare bármilyen Office 365-alkalmazást a felhasználóival az Azure RemoteApp segítségével."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 140056c22c8c69b9ec605318e35a72b144da07eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Get hello azonos Office 365-élmény bármely Azure RemoteApp eszközön
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ez a cikk hogyan toodeploy Office 365 a vállalat bármely eszközön. A felhasználók férhetnek hello ugyanazokat a képességeket, és Android-, Apple- és Windows felhasználói felület tapasztalattal.

Ezt az Azure RemoteApp használatával érheti el az Office 365 méretezhető virtuális gépeken való üzemeltetésével az Azure-ban, amelyekhez a felhasználók csatlakozhatnak. A virtuális gépek ezen készletét „felhőalapú gyűjteménynek” nevezik.

## <a name="create-a-cloud-collection"></a>Felhőalapú gyűjtemény létrehozása
Először az Azure-fiók létrehozását követően nyissa meg túl**RemoteApp** bal oldali hello hello hivatkozásra kattintva.
![Azure RemoteApp ábrázoló hello Azure portál](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Folytassa a kattintva **új** hello alsó és a "gyors létrehozása" gyűjteménye. Adjon meg nevet, hello régió, hello előfizetés, hello terv és hello kép "Office Professional 2013", amely a Microsoft biztosítja.
![Párbeszédpanel létrehozása](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Miután befejezte a hello űrlap hello gyűjtemény létrehozásának folyamata elindul. A is tarthat tooan órát.

![Várakozás](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Miután hello folyamat befejeződik, fog megjelenni ehhez hasonló. Ha a **Publishing** (Közzététel) elemre kattint, láthatja, hogy a legtöbb Office-alkalmazás már közzé van téve.
![Létrehozott gyűjtemény](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Közzétett alkalmazások](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Ezen a ponton is hozzáadhat további felhasználóknak, akik számára hozzáférést toothis gyűjtemény kattintva **felhasználói hozzáférés**.
![Felhasználói hozzáférés konfigurálása](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Most próbáljon meg csatlakozni tooOffice 365!

## <a name="connect-toooffice-365"></a>Csatlakozás tooOffice 365
Azt fogja látogasson túl[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), görgessen lefelé, és kattintson a **le ügyfeleket** tooinstall hello Azure RemoteApp-ügyfelet a hello eszközön. hello az alábbi képernyőképek a Windowsra vonatkoznak.

Hello alkalmazás indítása után meg kell adnia toosign be a Microsoft-fiókjával (korábban "Live ID"), használjon most hello ugyanazt, mint amit az Azure-fiókjával. Amikor bejelentkezik, egy értesítés jelenik meg az új meghívókról, kattintson rá, ekkor az alábbihoz hasonló listának kell megjelennie. Fogadja el a hello meghívóra, amelyben az Azure-fiók tulajdonosának e-mail címe.

![Új meghívó](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Így néz ki, ha vannak új meghívók.

![Egy alkalmazás elfogadása](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Hello meghívó elfogadása után megtekintheti az összes hello Office-alkalmazásokba hello Azure RemoteApp-ügyfelet.

![Alkalmazások listája](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Elemre bármelyik ezek hello alkalmazás elindul az hello Azure virtuális gép, és minden beállítás el! Jó munkát!

![indítás](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

