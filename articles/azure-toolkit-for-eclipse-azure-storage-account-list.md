---
title: "Az Azure Storage-fiók listája"
description: "Az Azure-eszközkészlet használata az eclipse-ben a tárolási Fiókbeállítások kezelése"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a>Az Azure Storage-fiók listája
Az Azure storage-fiókok a JDK, a kiszolgáló és a tetszőleges összetevők, valamint állapot tárolására, gyorsítótárazás használata esetén használandó letöltési helyeiről engedélyezése. Eclipse-ben, amely a projektekhez az Eclipse munkaterületen elérhető ismert storage-fiókok listáját. Megnyitásához a **Tárfiókok** párbeszédpanel, amely segítségével kezelhető a lista belül eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Tárfiókok**.

Az alábbi látható a **Tárfiókok** párbeszédpanel.

![][ic719496]

Ezen a párbeszédpanelen is megnyithatja az egy **fiókok** párbeszédpanelek, amelyek használják a storage-fiókok, például a következő hivatkozásra:

* A **JDK** lapján a **kiszolgálókonfiguráció** párbeszédpanel.
* A **Server** lapján a **kiszolgálókonfiguráció** párbeszédpanel.
* A **összetevő felvétele** párbeszédpanel.
* A **gyorsítótárazását** tulajdonságainak párbeszédpanelét jeleníti meg.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>A storage-fiókok használatával egy közzétételi beállítások fájlja importálása
1. Belül a **Tárfiókok** párbeszédpanel, kattintson a **közzététele beállításfájl importálás**.

2. (Kihagyhatja ezt a lépést, ha már mentette a közzétételi beállítások fájlja a helyi számítógépen.) Az a **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**. Ha még nem jelentkezik be az Azure-fiókjába, fogja kérni fogja a bejelentkezéshez. Majd kéri az Azure mentési beállítások fájl közzététele. (Az eredményül kapott utasításokat a bejelentkezési oldalon - figyelmen kívül hagyhatja ezeket az Azure portál által szolgáltatott és Visual Studio felhasználóknak.) Mentse a helyi számítógépen.

3. Még mindig a **importálási előfizetési adatok** párbeszédpanel, kattintson a **Tallózás** gombra, válassza ki a korábban mentett helyileg közzétételi beállítások fájlja, majd **nyissa meg a**.

4. Kattintson a **OK** bezárásához a **importálási előfizetési adatok** párbeszédpanel.

## <a name="to-create-a-new-storage-account"></a>Egy új tárfiók létrehozása
1. Belül a **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.

2. Belül a **Tárfiók hozzáadása** párbeszédpanel, kattintson a **új**.

3. Belül a **új Tárfiók** párbeszédpanelen adja meg a következő értékeket:

   * Tárfiók neve.

   * A tárfiók helye.

   * A tárfiók leírását.

   * Az előfizetést, amelyhez a storage-fiók tartozik.

4. Kattintson a **OK** bezárásához a **új Tárfiók** párbeszédpanel.

A tárfiók keletkezik több percig is eltarthat. Létrehozása után kattintson **OK** bezárásához a **Tárfiók hozzáadása** párbeszédpanel, és az új tárfiók nem kerülnek be a rendelkezésre álló tár fiókok listáját.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Meglévő tárfiók hozzáadása a listához
1. Ha még nem rendelkezik Azure-tárfiókot, hozzon létre egyet a felsorolt lépéseket követve a **létrehozni egy új tárolási fiók szakasz** felett. (Másik lehetőségként létrehozhat egy új tárfiókot, a [Azure felügyeleti portálon][Azure Management Portal].)

2. Belül a **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.

3. Belül a **Tárfiók hozzáadása** párbeszédpanelen adja meg az értékeket **neve** és **hozzáférési kulcs**. A fiók nevét és hívóbetűjét egy meglévő Azure-tárfiókot kell lennie. Használja a **tárolási** szakasza a [Azure felügyeleti portálon] [ Azure Management Portal] a tárfiókok neve és kulcsok megtekintéséhez. A **Tárfiók hozzáadása** párbeszédpanel az alábbihoz hasonlóan fog kinézni.
   
   ![][ic719497]

4. Kattintson a **OK** bezárásához a **Tárfiók hozzáadása** párbeszédpanel.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Új hozzáférési kulcs használatához a tárfiók módosítása
1. Belül a **Tárfiókok** párbeszédpanel, kattintson a tároló fiókra szerkesztése szeretne, és kattintson **szerkesztése**.

2. Belül a **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanelen módosítsa a **hozzáférési kulcs** érték.

3. Kattintson a **OK** bezárásához a **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanel.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>A tárfiók eltávolítása a listáról, az eclipse-ben karbantartása
1. Belül a **Tárfiókok** párbeszédpanel, kattintson a tároló fiókra szerkesztése szeretne, és kattintson **eltávolítása**.

2. Kattintson a **OK** amikor a rendszer kéri a tárfiók eltávolítása.

> [!NOTE]
> A tárfiók keresztül eltávolítása a **Tárfiókok** párbeszédpanel csak eltávolítja azt a listában tárfiókok megtekinthető Eclipse belül. A tárfiók nem távolítja el az Azure-előfizetésből. Emellett a tárfiók sikerült listájában jelennek meg újra a után Eclipse Újratölti az előfizetés részleteit.
> 
> 

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
