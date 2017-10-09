---
title: "egy Azure Media Services-fiók használatával Aspera aaaUpload fájlok |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello fájlok feltöltése, amelyek segítségével hello Media Services-fiók van hozzárendelve a ** Aspera kiszolgáló az igény szerinti ** Azure szolgáltatást."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a>Fájlok feltöltése a hello Aspera Server igény szerinti szolgáltatás használata az Azure Media Services-fiók

## <a name="overview"></a>Áttekintés

Az **Aspera** egy nagy sebességű fájlátvitelre szolgáló szoftver. Az Azure **Aspera Server On Demand** szolgáltatása lehetővé teszi a nagy fájlok nagy sebességű fel- és letöltését közvetlenül az Azure Blob objektumtárba. További információ **Aspera igény szerinti**, lásd: hello [Aspera felhő](http://cloud.asperasoft.com/) hely. 
  
**Az igény szerinti Server Aspera** Azure: hello vásárolni [Azure piactér](https://azure.microsoft.com/en-us/marketplace/). A rendezés toocomplete vásárol **Aspera Server igény szerinti** az Azure-ba, jelentkezzen be Windows Live ID azonosítójával. az Azure piactéren

Ez az oktatóanyag végigvezeti hello fájlok feltöltése, amelyek segítségével hello Media Services-fiók van hozzárendelve a **Aspera Server igény szerinti** Azure-szolgáltatás. 

Példa bemutatja, hogyan toouse Azure functions Aspera és a Media Services található [Itt](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).

>[!NOTE]
>Nincs az Azure Media Services feldolgozó media processzor (felügyeleti csomagok) támogatott korlátot toohello fájl maximális méretét. Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.
>

## <a name="prerequisites"></a>Előfeltételek 

toocomplete ebben az oktatóanyagban szüksége:

* Egy Windows Live ID
* Egy [Azure-fiók](https://azure.microsoft.com). További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
* Egy [Azure Media Services-fiók](media-services-portal-create-account.md).

## <a name="purchase-aspera-on-demand-for-azure"></a>Az Azure-hoz készült Aspera On Demand megvásárlása

Amennyiben az Azure piactéren jelentkezett, kövesse az alábbi alapvető lépéseket toocomplete vásárlásakor Aspera igény szerinti az Azure.

1. Keressen rá az Aspera kifejezésre, és válassza a „Server On Demand” lehetőséget.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. Tekintse át a hello előfizetési csomag, majd kattintson a "Sign Up"

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. Töltse ki hello részletekről a kiszolgáló igény szerint az előfizetésben.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. Kattintson a hello **Tarifacsomagot** és hello sub panelen jelölje ki a kívánt havi kötetre. A hello **részletek megtervezése** panelen, jelölje be **OK**. Ezt követően a hello **válassza ki a Tarifacsomagot** panelen, kattintson a **válasszon**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. Kattintson a **jogi feltételeket** tooview, és fogadja el a jogi feltételeket hello hello sub panelen. Miután átolvasta hello jogi feltételeket, kattintson a **beszerzési**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. Hello megvásárolhatja kattintva **létrehozása**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. hello Azure irányítópult fog értesíthetik hello szolgáltatást a(z) kiépítése.  Ha szolgáltatáskiépítéssel befejeződött, megtalálhatja hello új előfizetés hello hello társított szolgáltatás neve az erőforrások megkeresése. Miután megtalálta hello szolgáltatást, a dupla kattintás toolaunch hello Szolgáltatáskezelési portál.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. Indítsa el a hello Aspera felügyeleti portálon. Miután megtalálta az új Aspera szolgáltatást, hello szolgáltatás kattintva áttekintésével felmérheti, hozzáférési toohello felügyeleti portálon.  Ekkor egy új panel jelenik meg. A belül, hogy új panel van szüksége a hello tooclick **erőforrásnév** az új szolgáltatás.  A következő képernyőkép hello hello erőforrás neve: "AsperaTransferDemo". Miután a hello erőforrás nevére kattint, egy másik panel nincs elindítva. Az újonnan megjelenő panelen látható a „Felügyelet” hivatkozás. Kattintson a hello "Kezelése" hivatkozást toolaunch hello Aspera felügyeleti portálon.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. Hello kattintva kezelése hivatkozásra, elérhetővé válik a toohello regisztrációs lapjához, amely pedig szükséges tooaccess hello szolgáltatást.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. Ezen a ponton rendelkeznie kell hozzáféréssel toohello Aspera felügyeleti portálon, ahol elérési kulcsok létrehozása, letöltheti Aspera ügyfelek és a licencek, tekintse meg és API-k hello megismerése.

    hello alábbi képernyőfelvételen látható hello hozzáférés létrehozását. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    hello alábbi képernyőfelvételen látható hello használati hello portálon felületek jelentéskészítés. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a>Fájlok feltöltése az Asperával

1. Töltse le és telepítse hello Aspera ügyfélszoftvert:
    
    * [Böngészőbővítmény](http://downloads.asperasoft.com/connect2/)
    * [Funkciógazdag ügyfél](http://downloads.asperasoft.com/en/downloads/2)

2. Hajtsa végre az első átvitelt. A sorrend toouse hello Aspera ügyfél tootransfer hello Aspera adatátviteli szolgáltatás a toocomplete hello következőkre lesz szüksége: 

    1. Hívóbetű, hello Aspera portálon létrehozásához.  
    2. Letöltése, telepítése és licenc hello Aspera ügyfél (szoftver hello Aspera portálon találhatók).  

    >[!NOTE]
    >Kérjük, olvassa el a hello Aspera ügyfél útmutató a konfigurációs adatokat.
    
    3. Néhány adatbeolvasás a tárfiókja, amely kapcsolódik az Azure Media fiókja hello segítségével [Azure-portálon](https://portal.azure.com/). Pontosabban, a neve, a kulcs és a hello storage blob-tároló nevének toowhich érdemes tooplace a tartalmat. 

        * tooget hello tárolási info hello portálról: található a tárfiók, kattintson a hello elérési kulcsok és a példány hello nevét és a hello fiókja kulcsot.
        * tooget hello tároló neve: található a tárfiók, jelölje be **Blobok**, jelölje be a tooupload hello tartalom hello tároló hello neve. 

    Az alábbiakban van hello képernyőfelvétel a hello Aspera ügyfél **Csatlakozáskezelő** ahol meg kell adnia hello "Azure" tárolási típusa és a hitelesítő adatokat, valamint a hello blob tároló.

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a>Erőforrások

a következő erőforrások hello ebben a cikkben említett. 

* [Böngészőbővítmény csatlakoztatása](http://downloads.asperasoft.com/connect2/)
* [Csatlakoztatási útmutató](http://downloads.asperasoft.com/en/documentation/8)
* [Aspera-ügyfél](http://downloads.asperasoft.com/en/downloads/2)
* [Ügyfélútmutató](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a>Következő lépések

Most már tudja, hogyan [másolhat blobokat egy tárfiókból egy AMS-fiókba](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

