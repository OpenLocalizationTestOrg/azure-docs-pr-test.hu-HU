---
title: "aaaMicrosoft Azure StorSimple és a felhőalapú megoldások szolgáltató Program áttekintése |} Microsoft Docs"
description: "Áttekintése hello StorSimple és a CSP a StorSimple-partnerek számára."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: b5d999f2fbb9a27e7404ff454957b29dbef56af6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a>Cloud Solution Provider programot a StorSimple virtuális tömb telepíteni

## <a name="overview"></a>Áttekintés

A StorSimple virtuális tömb hello Cloud Solution Provider (CSP) partnerek az ügyfelek számára telepíthető. Kriptográfiai Szolgáltató partner hozhat létre a StorSimple eszköz Manager szolgáltatást. Ez a szolgáltatás majd használt toodeploy kell és kezelheti a StorSimple virtuális tömb és hello tartozó megosztások, kötetek és biztonsági másolatokat.

Ez a cikk ismerteti, hogyan CSP partner hozzáadása az ügyfél vagy egy új előfizetés tooan meglévő ügyfél és majd hozzon létre egy szolgáltatás toodeploy egy StorSimple virtuális tömb CSP-hez.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, győződjön meg arról, hogy:

- A hello CSP program regisztrált.
- Rendelkezik érvényes [Partnerközpontjában](http://partnercenter.microsoft.com/) bejelentkezési hitelesítő adatokat. hello hitelesítő adatok lehetővé teszik a toohello Partner portál tooadd új ügyfelek toosign, keresse meg az ügyfelek vagy tooa felhasználói fiók hello Partner irányítópultról lépjen. hello CSP működhessen, a StorSimple rendszergazda hello Azure-portálon a hello ügyfél nevében.
                             
## <a name="add-a-customer"></a>Az ügyfél hozzáadása

Ha ad hozzá, az ügyfél, a rendszer automatikusan létrehoz egy előfizetést. az ügyfél tooadd (és automatikusan az előfizetés létrehozása), hajtsa végre a következő lépéseket hello partnerportálon hello.

1. Nyissa meg toohello [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival. Kattintson a **irányítópult**.

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. A hello bal oldali ablaktáblában kattintson **ügyfelek**. Hello jobb oldali ablaktáblában kattintson **adja hozzá az ügyfelek**. Írja be a hello ügyfél hello adatait. Kattintson a **tovább: előfizetések** toocreate egy ügyfél-előfizetést.

    ![Ügyfél hozzáadása](./media/storsimple-partner-csp-deploy/image2.png)

3.  Válassza ki **Microsoft Azure** kínálnak. Görgessen toohello alsó hello lap, és kattintson a **felülvizsgálati**.

    ![Tekintse át az előfizetési adatok](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. Tekintse át hello információit, és kattintson a **Submit**.

    ![Küldje el az előfizetéshez](./media/storsimple-partner-csp-deploy/image4.png)

5. Mentse a hello megerősítő információ későbbi felhasználás céljából.

    ![Mentés megerősítése](./media/storsimple-partner-csp-deploy/image5.png)

6. Található, vagy keresse meg a hozzáadott toohello ügyfél. Kattintson a hello **vállalatnév** hello részleteinek le toodrill.

    ![Hello ügyfél keresése](./media/storsimple-partner-csp-deploy/image6.png)  

7. A hello bal oldali ablaktáblában jelölje ki a **szolgáltatásfelügyelet**. A hello jobb oldali ablaktáblában, a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** toosign az Azure rendszergazdaként az ügyfél számára.

    ![Jelentkezzen be tooAzure portál](./media/storsimple-partner-csp-deploy/image9.png)

8. toocreate egy StorSimple-kezelőt, kattintson a **+ új** , és keresse meg vagy nyissa meg a túl**StorSimple virtuális eszköz adatsorozat**. További információ: túl[StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a>Előfizetés hozzáadása

Bizonyos esetekben előfordulhat, hogy egy meglévő ügyfél, és tooadd előfizetés van szüksége. tooadd előfizetés tooan meglévő ügyfél, hajtsa végre a lépéseket hello partnerportálon hello.

1. Nyissa meg toohello [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival. Kattintson a **irányítópult**.

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. A hello bal oldali ablaktáblában kattintson **ügyfelek**. Található, vagy keresse meg az előfizetés tooadd kívánt toohello ügyfél. Kattintson a hello ![kibontott pipa ikon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikon tooexpand hello sor hello vállalat nevét az ügyfél számára. Hello részleteit, kattintson a **előfizetéseket**.

    ![Ügyfelek](./media/storsimple-partner-csp-deploy/image10.png)

3. Ellenőrizze **Microsoft Azure** a hello **ajánlatok felső** hello előfizetés, és kattintson a **Submit**. Ez egy új előfizetést hoz létre.

    ![Új előfizetés hozzáadása](./media/storsimple-partner-csp-deploy/image11.png)

6. Új előfizetés létrehozása után kattintson **<--ügyfelek** a hello bal oldali ablaktáblán tooreturn toohello **ügyfelek** lap. Keresse meg, amely most létrehozott előfizetés hello ügyfél. Kattintson a hello **vállalatnév** hello részleteinek le toodrill.

    ![Hello ügyfél keresése](./media/storsimple-partner-csp-deploy/image6.png)  

7. A hello bal oldali ablaktáblában jelölje ki a **szolgáltatásfelügyelet**. A hello jobb oldali ablaktáblában, a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** toosign az Azure rendszergazdaként az ügyfél számára.

    ![Jelentkezzen be tooAzure portál](./media/storsimple-partner-csp-deploy/image9.png)

8. toocreate egy StorSimple-kezelőt, kattintson a **+ új** , és keresse meg vagy nyissa meg a túl**StorSimple virtuális eszköz adatsorozat**. További információ: túl[StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a>Következő lépések

- Ha kérdései további hello StorSimple a CSP-hez, folytassa az túl[CSP a StorSimple: gyakran ismételt kérdések](storsimple-partner-csp-faq.md).
- Ha kész toodeploy a StorSimple, nyissa meg túl[központi telepítése a CSP a StorSimple](storsimple-partner-csp-deploy.md).
