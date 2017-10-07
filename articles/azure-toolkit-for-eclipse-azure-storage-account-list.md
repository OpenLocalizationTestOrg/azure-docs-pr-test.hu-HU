---
title: "Tárfiók aaaAzure"
description: "Az Eclipse hello Azure eszközkészlet használata tárolási beállításainak kezelése"
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
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a>Az Azure Storage-fiók listája
Az Azure storage-fiókok lehetővé teszik a JDK, a kiszolgáló és a tetszőleges összetevők, valamint a gyorsítótárazás használatakor állapot tárolásához használt helyek toobe letöltése. Eclipse-ben elérhető tooyour projektekhez az Eclipse munkaterületen ismert storage-fiókok listáját. tooopen hello **Tárfiókok** párbeszédpanel, amely, amely a listában, belül eclipse-ben használt toomanage kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure** , és kattintson a **Tárfiókok**.

hello következő mutatja hello **Tárfiókok** párbeszédpanel.

![][ic719496]

Ezen a párbeszédpanelen is megnyithatja az egy **fiókok** storage-fiókok, például hello következő használó párbeszédpanelek hivatkozásra:

* Hello **JDK** hello lapján **kiszolgálókonfiguráció** párbeszédpanel.
* Hello **Server** hello lapján **kiszolgálókonfiguráció** párbeszédpanel.
* Hello **összetevő felvétele** párbeszédpanel.
* Hello **gyorsítótárazását** tulajdonságainak párbeszédpanelét jeleníti meg.

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a>a tárolási fiókok tooimport a közzétételi beállítások fájlja
1. Hello belül **Tárfiókok** párbeszédpanel, kattintson a **közzététele beállításfájl importálás**.

2. (Kihagyhatja ezt a lépést, ha már mentette a közzétételi beállítások fájl tooyour helyi számítógépen.) A hello **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**. Ha még nem jelentkezik be az Azure-fiókjába, a kért toolog fogja. Akkor kérni fogja az Azure toosave közzététele beállításfájl. (Hello eredményül kapott utasításokat az hello bejelentkezési oldalakon - figyelmen kívül hagyhatja ezeket hello Azure-portál által biztosított és a Visual Studio felhasználóknak.) Mentse a helyi számítógép tooyour.

3. Továbbra is a hello **importálási előfizetési adatok** párbeszédpanel, hello kattintson **Tallózás** gombra, jelölje be hello közzététele beállításfájl korábban mentett helyileg, és kattintson a **megnyitása**.

4. Kattintson a **OK** tooclose hello **importálási előfizetési adatok** párbeszédpanel.

## <a name="toocreate-a-new-storage-account"></a>új tárfiók toocreate
1. Hello belül **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.

2. Hello belül **Tárfiók hozzáadása** párbeszédpanel, kattintson a **új**.

3. Hello belül **új Tárfiók** párbeszédpanelen adható meg érték a következő hello:

   * Tárfiók neve.

   * Hello tárfiók helye.

   * Hello tárfiók leírását.

   * hello előfizetés toowhich hello tárfiók tartozik.

4. Kattintson a **OK** tooclose hello **új Tárfiók** párbeszédpanel.

A tárolási fiók toobe létrehozása több percig is eltarthat. Kattintson a létrehozás után **OK** tooclose hello **Tárfiók hozzáadása** párbeszédpanel, és az új tárfiók hozzáadódnak toohello rendelkezésre álló tár listája.

## <a name="tooadd-an-existing-storage-account-toohello-list"></a>egy meglévő tárolási toohello fióklista tooadd
1. Ha még nem rendelkezik egy fiókot, hozzon létre egyet által az Azure storage hello lépések felsorolt hello **toocreate új tárolási fiók szakasz** felett. (Másik lehetőségként létrehozhat egy új tárfiókot hello, [Azure felügyeleti portálon][Azure Management Portal].)

2. Hello belül **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.

3. Hello belül **Tárfiók hozzáadása** párbeszédpanelen adja meg az értékeket **neve** és **hozzáférési kulcs**. hello nevét és a hozzáférési kulcsot a meglévő Azure-tárfiók kell lennie. Használjon hello **tárolási** hello szakasza [Azure felügyeleti portálon] [ Azure Management Portal] tooview a tárfiók nevét és a kulcsok. A **Tárfiók hozzáadása** párbeszédpanel hasonló toohello következő fog kinézni.
   
   ![][ic719497]

4. Kattintson a **OK** tooclose hello **Tárfiók hozzáadása** párbeszédpanel.

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a>a tárolási fiók toouse új hozzáférési kulcs toomodify
1. Hello belül **Tárfiókok** párbeszédpanel, kattintson a tárfiók hello tooedit szeretne, és kattintson a **szerkesztése**.

2. Hello belül **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanelen hello módosítása **hozzáférési kulcs** érték.

3. Kattintson a **OK** tooclose hello **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanel.

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a>tooremove hello listából tárfiók tartani az eclipse-ben
1. Hello belül **Tárfiókok** párbeszédpanel, kattintson a tárfiók hello tooedit szeretne, és kattintson a **eltávolítása**.

2. Kattintson a **OK** amikor felszólító tooremove hello storage-fiók.

> [!NOTE]
> Hello keresztül hello tárfiók eltávolítása **Tárfiókok** párbeszédpanel csak eltávolítja hello listája megtekinthető Eclipse belül. Hello tárfiók nem távolítja el az Azure-előfizetésből. Emellett hello tárfiók sikerült listájában jelennek meg újra a után Eclipse Újratölti az előfizetés hello részleteit.
> 
> 

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
