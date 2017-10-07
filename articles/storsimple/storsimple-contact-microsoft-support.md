---
title: "a StorSimple 8000 series támogatási jegy aaaLog |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate támogatási kérelem és a StorSimple eszköz támogatási munkamenet indításához."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>A StorSimple a Microsoft támogatási szolgálatához
Ha problémába ütközik a Microsoft Azure StorSimple megoldás, létrehozhat egy szolgáltatási kérelem a technikai támogatási szolgálathoz. Egy online munkamenetben a támogatási szakértőhöz szükség lehet a támogatási munkamenet toostart a StorSimple eszköz. Ez a cikk bemutatja, hogyan:

* Hogyan toocreate támogatási kérelmet.
* A támogatási munkamenet toostart hogyan Windows PowerShell felületet a StorSimple eszköz hello.

Felülvizsgálati hello [a StorSimple 8000 Series támogatási SLA-k és információk](https://msdn.microsoft.com/library/mt433077.aspx) a támogatási kérelem létrehozása előtt.

## <a name="create-a-support-request"></a>Támogatási kérés létrehozása
Hajtsa végre a következő lépéseket toocreate egy támogatási kérést hello:

#### <a name="toocreate-a-support-request"></a>egy támogatási kérést toocreate
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/)hello jobb felső sarokban, kattintson a fiók nevére, kattintson **forduljon a Microsoft ügyfélszolgálatához**.
   
    ![Forduljon az MS-támogatás ManagementPortal keresztül](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Átirányított toohello új Azure-portálon (portal.azure.com) lesz. Kattintson a hello **új támogatja a kérelem** csempére.
   
    ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Üdvözlő képernyőt hello jobb oldalán, hello **új támogatja a kérelem** ablaktáblán jelenik meg. 
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. A hello **alapjai** teljes hello a következő párbeszédpanelen:                                
   
   1. A hello **típusú** legördülő listában válassza **műszaki**.
   2. Válassza ki a **előfizetés** hello legördülő listából.
   3. A hello **szolgáltatás** legördülő listában válassza **StorSimple**. 
   4. Válassza ki a **támogatási csomag** hello legördülő listából. A technikai támogatási szolgálatával fizetős támogatási terv tooenable van szüksége.
4. Kattintson a **Tovább** gombra. Hello **probléma** párbeszédpanel jelenik meg.
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. A hello **probléma** teljes hello a következő párbeszédpanelen:
   
   1. Válassza ki a **súlyossági** szintű hello legördülő listából.
   2. Válassza ki a **problématípust** hello legördülő listából.
   3. Válassza ki a **kategória** hello legördülő listából. 
   4. A hello **részletek** mezőben röviden ismertesse a problémát.
   5. A hello **időkereten belül** hello dátum, idő és időzóna, a probléma legutóbbi előfordulásának toohello megfelelő mezőbe.
   6. A **fájlfeltöltés**, kattintson a hello mappa ikon toobrowse tooyour támogatási csomag.
   7. Jelölje be hello **diagnosztikai információkat** jelölőnégyzetet.
6. Kattintson a **Tovább** gombra. Hello **elérhetőségi adatai** párbeszédpanel jelenik meg.
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Adja meg a kapcsolattartási adatait, és válasszon ki egy kapcsolattartási módszert, (telefonon vagy e-mailek). 
8. Jelölje be hello **kapcsolattartási adatok módosításainak mentése az a későbbi támogatási kérelmekhez** jelölőnégyzetet.
9. Kattintson a **Create** (Létrehozás) gombra.

Miután elküldte a kérést, a támogatási szakértőhöz felveszi Önnel a lehető leghamarabb tooproceed a kérelemhez.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Támogatási munkamenetet indítani a Windows PowerShellben a StorSimple
bármely ad ki, hogy tootroubleshoot hello StorSimple eszköz tapasztalhat, hello Microsoft Support csapattal tooengage kell. Előfordulhat, hogy a Microsoft Support toouse egy támogatási munkamenet toolog tooyour eszközön. 

Hajtsa végre a következő hello támogatási munkamenet toostart lépések:

#### <a name="toostart-a-support-session"></a>egy támogatási munkamenet toostart
1. Hozzáférés hello eszköz általi közvetlen soros konzolon hello vagy egy telnet-munkamenet távoli számítógépről. toodo a, hajtsa végre hello lépéseit [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. A megnyitott munkamenetben hello, nyomja le a hello **Enter** kulcs tooget egy parancssori ablakot.
3. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.
4. Hello parancssorba írja be a következő jelszó hello: 
   
    `Password1`
5. Hello parancssorba írja be a következő parancs hello:
   
    `Enable-HcsSupportAccess`
6. Egy titkosított karakterláncot tooyou jelenik. Másolja ezt a karakterláncot egy szövegszerkesztőbe, például a Jegyzettömböt.
7. Mentse ezt a karakterláncot, és küldje el a egy e-mail üzenet tooMicrosoft támogatása. 

> [!IMPORTANT]
> Letilthatja a támogatás elérését futtatásával `Disable-HcsSupportAccess`. hello StorSimple eszközt is megpróbál toodisable támogatási hozzáférés 8 órával azt követően hello munkamenetet kezdeményezett. A bevált gyakorlat toochange a StorSimple eszköz felhasználó hitelesítő adatait, a támogatási munkamenet kezdeményezése után.
> 
> 

