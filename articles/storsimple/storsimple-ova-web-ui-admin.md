---
title: "aaaStorSimple virtuális tömb webes felhasználói felület felügyeleti |} Microsoft Docs"
description: "Ismerteti, hogyan tooperform alapvető eszköz felügyeleti feladatok hello StorSimple virtuális tömb webes felhasználói felületen keresztül."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a>Használja a következő hello webes felhasználói felületén tooadminister a StorSimple virtuális tömböt
![a telepítő folyamatábra](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Áttekintés
hello oktatóanyagok ebben a cikkben toohello Microsoft Azure StorSimple virtuális tömb (más néven hello StorSimple helyszíni virtuális eszköz) futó 2016. március általános elérhetőségével (GA) kiadás vonatkoznak. Ez a cikk ismerteti az egyes hello bonyolult munkafolyamatok és a StorSimple virtuális tömb hello végrehajtható felügyeleti feladatokhoz. Kezelheti a StorSimple virtuális tömb hello StorSimple Manager szolgáltatás (hivatkozott tooas hello portál felhasználói felületének) felhasználói felület használatával hello és helyi webes felhasználói felület hello eszköz hello. Ez a cikk foglalkozik hello feladatok végrehajtható hello webes felhasználói felület használatával.

Ez a cikk a következő oktatóanyagok hello tartalmazza:

* Hello szolgáltatásadat-titkosítási kulcs beszerzése
* Webes felhasználói felület telepítő hibák elhárítása
* A napló-csomag létrehozása
* Állítsa le és indítsa újra az eszközt

## <a name="get-hello-service-data-encryption-key"></a>Hello szolgáltatásadat-titkosítási kulcs beszerzése
A szolgáltatásadat-titkosítási kulcs jön létre, amikor az első eszköz regisztrálása a StorSimple Manager szolgáltatás hello. Ezt a kulcsot meg kell majd hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Manager szolgáltatás szükséges.

Ha rendelkezik a szolgáltatásadat-titkosítási kulcs helytelen, és tooretrieve kell azt, tegye a következőket hello lépéseket hello helyi webes felhasználói felület hello eszköz regisztrálva a szolgáltatásban.

#### <a name="tooget-hello-service-data-encryption-key"></a>tooget hello szolgáltatásadat-titkosítási kulcs
1. Csatlakozás toohello helyi webes felhasználói felület. Nyissa meg túl**konfigurációs** > **Felhőbeállítások**.
2. Hello a hello lap alján, kattintson **Get szolgáltatásadat-titkosítási kulcs**. A kulcs jelenik meg. Másolja ki és mentse ezt a kulcsot.
   
    ![szolgáltatásadat-titkosítási kulcs 1 beolvasása](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Webes felhasználói felület telepítő hibák elhárítása
Bizonyos esetekben hello helyi webes felhasználói felületén keresztül hello eszköz konfigurálásakor mutatjuk be hibák. toodiagnose és ilyen hibák elhárításával kapcsolatos tudnivalókat hello diagnosztikai tesztek futtatása.

#### <a name="toorun-hello-diagnostic-tests"></a>toorun hello diagnosztikai tesztek
1. Hello helyi webes felhasználói felületén, nyissa meg túl**hibaelhárítás** > **diagnosztikai tesztek**.
   
    ![1 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image29.png)
2. Hello a hello lap alján, kattintson **diagnosztikai tesztek futtatása**. A szolgáltatás kezdeményez tesztek toodiagnose a hálózati, eszköz, webalkalmazás-proxy, idő vagy felhőbeállítások minden lehetséges problémákat. Értesítést fog kapni, hogy hello eszköz teszteket futtat.
3. Miután hello teszt befejeződött, hello eredmény jelenik meg. hello következő példa bemutatja hello diagnosztikai tesztek eredményét. Vegye figyelembe, hogy hello webproxy beállításai nem lettek konfigurálva az eszközön, és ezért hello webalkalmazás-proxy teszt nem futott. Minden egyéb hálózati beállítások, a DNS-kiszolgáló tesztek hello és időbeállítást sikeres volt.
   
    ![2 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>A napló-csomag létrehozása
A napló csomag, amely segíthet a Microsoft Support bármely eszköz problémák elhárítása az összes hello megfelelő napló magában foglalja. Ebben a kiadásban a napló csomag is generálható hello helyi webes felhasználói felületen keresztül.

#### <a name="toogenerate-hello-log-package"></a>toogenerate hello napló csomag
1. Hello helyi webes felhasználói felületén, nyissa meg túl**hibaelhárítás** > **rendszernaplókat**.
   
    ![napló csomagot 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. Hello a hello lap alján, kattintson **napló-csomag létrehozása**. A csomag hello rendszer naplók jön létre. Ez eltarthat néhány percig.
   
    ![napló csomagot 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Értesítést fog kapni után hello csomag sikeresen jön létre, és hello oldal frissül tooindicate hello és hello csomag létrehozásának időpontja.
   
    ![napló csomagot 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. Kattintson a **napló letöltőcsomag**. A tömörített csomag a rendszeren, tölti le a program.
   
    ![napló csomagot 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. Bontsa ki a letöltött napló csomag hello tekinthetők meg és hello rendszernapló fájljaiban.

## <a name="shut-down-and-restart-your-device"></a>Állítsa le és indítsa újra az eszközt
Állítsa le, vagy indítsa újra a virtuális eszköz hello helyi webes felhasználói felület használatával. Javasoljuk, hogy indítsa újra, hello kötetek vagy megosztások offline végrehajtása a hello állomás és az eszköz majd hello előtt. Ezzel minimálisra csökkentheti adatsérülés lehetőségét. 

#### <a name="tooshut-down-your-virtual-device"></a>tooshut le a virtuális eszköz
1. Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **energiaellátási beállítások**.
2. Hello a hello lap alján, kattintson **leállítási**.
   
    ![1 eszköz Leállítás](./media/storsimple-ova-web-ui-admin/image36.png)
3. Megjelenik egy figyelmeztetés, amely meghatározza, hogy, hogy a leállás hello eszköz meg fogja szakítani a bármely IO, amelyek folyamatban volt egy állásidővel. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![eszköz leállítás figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Értesítést fog kapni hello leállítás inicializálva lett.
   
    ![eszköz leállítás indítása](./media/storsimple-ova-web-ui-admin/image38.png)
   
    hello eszköz most leáll. Ha azt szeretné, toostart, szüksége lesz, amely a Hyper-V kezelője hello toodo.

#### <a name="toorestart-your-virtual-device"></a>toorestart a virtuális eszköz
1. Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **energiaellátási beállítások**.
2. Hello a hello lap alján, kattintson **indítsa újra a**.
   
    ![eszköz újraindítása](./media/storsimple-ova-web-ui-admin/image36.png)
3. Megjelenik egy figyelmeztetés, amely meghatározza, hogy újraindítás hello eszköz meg fogja szakítani a bármely IOs-, amelyek folyamatban volt egy állásidővel. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Indítsa újra a figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Értesítést fog kapni, hogy hello újraindítást kezdeményezett.
   
    ![újraindítást kezdeményezett](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Hello újraindítása folyamatban van, amíg hello kapcsolat toohello felhasználói felület elvész. Hello újraindítás végezhet figyelést, ha felhasználói felület hello rendszeresen frissíteni. Másik lehetőségként hello eszköz újraindítása állapotát a Hyper-V kezelője hello figyelheti.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás toomanage az eszköz](storsimple-virtual-array-manager-service-administration.md).

