---
title: "a Tártallózó (előzetes verzió) használatába aaaGet |} Microsoft Docs"
description: "Azure tárerőforrások kezelése a Tártallózó alkalmazással (előzetes verzió)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a>Ismerkedés a Tártallózó alkalmazással (előzetes verzió)
## <a name="overview"></a>Áttekintés
Az Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux. Ebből a cikkből megismerheti hello csatlakozás az Azure storage-fiókok kezelése tooand különféle módjait.

![Microsoft Azure Tártallózó (előzetes verzió)][15]

## <a name="prerequisites"></a>Előfeltételek
* [A Tártallózó (előzetes verzió) letöltése és telepítése](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a>Csatlakozás tooa tárfiók vagy szolgáltatás
A Tártallózó (előzetes verzió) számos lehetőséget biztosít tooconnect toostorage fiókok. Megteheti például a következőt:
* Csatlakozás az Azure-előfizetésekkel társított toostorage fiókokat.
* Csatlakoztatja a toostorage fiókokat és szolgáltatásokat, megosztott a többi Azure-előfizetések.
* Csatlakozás tooand helyi tárterület kezelése hello Azure Storage Emulator használatával. 

Emellett használhatja a tárfiókokat a globális és az országos Azure-ban:

* [Csatlakozás Azure-előfizetés tooan](#connect-to-an-azure-subscription): tooyour Azure-előfizetéséhez tartozó tárolási erőforrások kezelése.
* [Helyi fejlesztési tárolási együttműködve](#work-with-local-development-storage): helyi tárterület kezelése hello Azure Storage Emulator használatával.
* [Tooexternal tárolási csatolása](#attach-or-detach-an-external-storage-account): tooanother Azure-előfizetéséhez tartozó tárolási erőforrások kezelése vagy a nemzeti Azure felhők hello tárolási fiók nevét, a kulcs és a végpontok segítségével.
* [A storage-fiók csatolása a SAS használatával](#attach-storage-account-using-sas): a közös hozzáférésű jogosultságkód (SAS) használatával tooanother Azure-előfizetéséhez tartozó tárolási erőforrások kezelése.
* [Szolgáltatás csatolása a SAS használatával](#attach-service-using-sas): egy adott tárolási szolgáltatás (blob tároló, sor vagy tábla), amely tooanother Azure-előfizetés tartozik egy SAS használatával kezelheti.

## <a name="connect-tooan-azure-subscription"></a>Csatlakozás Azure-előfizetés tooan
> [!NOTE]
> Ha nincs Azure-fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) vagy [aktiválhatja Visual Studio-előfizetése kiemelt előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. A Tártallózóban (előzetes verzió) válassza ki az **Azure-fiók beállításai** lehetőséget.

    ![Azure-fiók beállításai][0]

2. hello bal oldali ablaktáblán megjelennek azok összes hello Microsoft-fiókkal jelentkezett be. tooconnect tooanother fiókját, válassza **vegyen fel egy fiókot**, és kövesse a hello utasításokat toosign be legalább egy aktív Azure-előfizetéssel társított Microsoft-fiókkal.

    >[!NOTE]
    >Csatlakozás Azure (például a Németországi Azure Azure Government és bejelentkezési keresztül Azure Kína) toonational jelenleg nem támogatott. Lásd: hello "Attach vagy külső tárfiók leválasztása" szakaszban arról, hogyan tooconnect toonational Azure storage-fiókok.

3. Miután sikeresen bejelentkezett Microsoft-fiókkal, hello bal oldali ablaktáblán a telepítéskor hello fiókhoz társított Azure-előfizetések. Válassza ki az Azure-előfizetések, amelyek szeretné, hogy a toowork, és válassza hello **alkalmaz**. (Kiválasztásával **előfizetéseket** kiválasztásával vagy hello váltógombok felsorolt Azure-előfizetések.)

    ![Azure-előfizetések kiválasztása][3]  
    hello bal oldali panelen kiválasztott hello Azure-előfizetéssel társított tárfiókokat hello jeleníti meg.

    ![Kiválasztott Azure-előfizetések][4]

## <a name="connect-tooan-azure-stack-subscription"></a>Csatlakozás tooan Azure verem előfizetés

Csatlakozás tooan Azure verem előfizetés kapcsolatos információkért lásd: [Tártallózó csatlakozás tooan Azure verem előfizetés](azure-stack/azure-stack-storage-connect-se.md).

## <a name="work-with-local-development-storage"></a>Munkavégzés helyi fejlesztési tárterülettel
A Tártallózó (előzetes verzió) használhat elleni helyi tároló hello Azure Storage Emulator használatával. Ez a megközelítés lehetővé teszi kódot a tárterületre és a vizsgálati tárolási írását nélkül feltétlenül egy tárfiókot, Azure-ban telepített mert hello tárfiók hello Azure Storage Emulator tárfiókra.

> [!NOTE]
> csak a Windows hello Azure Storage Emulator jelenleg támogatott.
>
>

1. Hello a Tártallózó (előzetes verzió) bal oldali ablaktáblán, bontsa ki a hello **(helyi és kapcsolódó)** > **Tárfiókok** > **(fejlesztés)** csomópont.

    ![Helyi fejlesztési csomópont][21]

2. Ha még nem telepítette hello Azure Storage Emulator,-e felszólító toodo Igen, az információs sávon. Ha hello információs sáv megjelenik, jelölje be **hello legújabb verzió letöltése**, majd telepítse a hello emulátor.

    ![Azure Storage Emulator letöltése prompt][22]

3. Hello emulátor telepítése után hozzon létre, és együttműködik a helyi blobokat, üzenetsorokat és táblákat. hogyan írja be mindegyik storage-fiók toowork, toolearn lásd: hello a következők egyikét:

    * [Azure Blob Storage-erőforrások kezelése](vs-azure-tools-storage-explorer-blobs.md)
    * Azure File Share Storage-erőforrások kezelése: *Hamarosan elérhető*
    * Azure Queue Storage-erőforrások kezelése: *Hamarosan elérhető*
    * Azure Table Storage-erőforrások kezelése: *Hamarosan elérhető*

## <a name="attach-or-detach-an-external-storage-account"></a>Külső tárfiók csatolása vagy leválasztása
A Tártallózó (előzetes verzió) csatolhat a szolgáltatáskéréshez tooexternal storage-fiókok, hogy a storage-fiókok azok könnyen megoszthatóak. Ez a szakasz azt ismerteti, hogyan tooattach too(and detach from) külső tárfiókok.

### <a name="get-hello-storage-account-credentials"></a>Hello tárfiók hitelesítő adatainak lekérése
külső tárfiók tooshare, az adott fiók tulajdonosának hello kell szereznie hello fiók hello hitelesítő adatokat (a fióknevet és a kulcs) és oszthat meg ezt az információt tooattach toothat (külső) fiók kívánó hello személlyel. Beszerezhet hello tárfiók hitelesítő adatainak hello Azure-portálon keresztül hello következő tevékenységek végrehajtásával:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Válassza a **Tallózás** lehetőséget.

3. Válassza a **Tárfiókok** lehetőséget.

4. A hello **Tárfiókok** panelen, a select hello kívánt tárfiókot.

5. A hello **beállítások** hello panelen kiválasztott tárfiók, jelölje be **hívóbetűk**.

    ![Hozzáférési kulcs lehetőség][5]

6. A hello **hívóbetűk** panelen, a Másolás hello **tárfióknév** és **key1** értékek toohello tárfiók csatolásához.

    ![Elérési kulcs][6]

### <a name="attach-tooan-external-storage-account"></a>Tooan külső tárfiók csatolása
tooattach tooan külső tárfiók, kell hello fiók nevét és a kulcsot. hello "Get hello tárfiók hitelesítő adatainak" szakasz ismerteti, hogyan ezeket az értékeket tooobtain hello Azure-portálon. Azonban a hello portal hello fiókkulcs nevezik **key1**. Ezért a Tártallózó (előzetes verzió) fiókkulcs kér, ahol megadhatja hello **key1** érték.

1. A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. Hello a **tooAzure tárolási csatlakozás** párbeszédpanel hello fiókkulcs adja meg (hello **key1** hello Azure-portálon értéket), majd válassza ki **következő**.

    > [!NOTE]
    > A tárfiók nemzeti Azure adhat meg hello tárolási kapcsolati karakterlánc. Tooconnect tooAzure Németország storage-fiókok, írja be például a következő: kapcsolati karakterláncok hasonló toohello következő: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<tárfiók kulcsa>
    >* EndpointSuffix=core.cloudapi.de
    
    >Hello kapcsolati karakterlánc beolvasása hello Azure portál a hello azonos hasonlóan hello "Hello tárfiók hitelesítő adatainak beolvasása" szakasz.

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. A hello **külső tárterület csatolása** párbeszédpanel hello **fióknév** mezőbe, írja be a tárfiók neve hello, adja meg az egyéb beállításait, és válassza **következő**.

    ![Külső tárterület csatolása párbeszédpanel][8]

4. A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy. Ha semmi toochange, jelölje be **vissza** és írja be újra a hello szükséges beállításokat. 

5. Kattintson a **Csatlakozás** gombra.

6. Miután sikeresen csatlakoztatva van, hello külső tárfiók jelenik meg, amely **(külső)** toohello tárfiók neve lesz hozzáfűzve.

    ![Külső tárfiók tooan kapcsolódás eredménye][9]

### <a name="detach-from-an-external-storage-account"></a>Külső tárfiók leválasztása
1. Kattintson a jobb gombbal a külső tárfiók hello, szeretné, hogy toodetach, és válassza **leválasztási**.

    ![Tár leválasztása lehetőség][10]

2. Hello megerősítő üzenetet, jelölje ki **Igen** hello külső tárfiók leválasztásának tooconfirm hello.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Tárfiók csatolása SAS használatával
Egy [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lehetővé teszi, hogy a Üdvözöljük a rendszergazdákat az Azure-előfizetés adjon ideiglenes hozzáférést tooa tárfiók tooprovide Azure-előfizetés hitelesítő adatok nélkül.

tooillustrate ebben az esetben most mondja ki, hogy "a" felhasználó Azure-előfizetés rendszergazdája, és "a" felhasználó szeretne tooallow "b" felhasználó tooaccess tárolási fiók adott időtartamra és meghatározott engedélyekkel:

1. "A" felhasználó időszak és hello szükséges engedélyekkel az adott időszak hoz létre egy SAS (álló hello tárfiók hello kapcsolati karakterlánc).

2. "A" felhasználó megosztások SAS hello személlyel hello ("b" felhasználó, a fenti példában) hozzáférési toohello tárfiók kívánó.  

3. "B" felhasználó a Tártallózó (előzetes verzió) tooattach toohello fiókot használja, amelyet a tooUserA tartozik hello megadott SAS.

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a>Az SAS lekérése hello fióknevet, amelyet az tooshare
1. A Tártallózó (előzetes verzió), kattintson a jobb gombbal szeretné osztani, majd válassza ki hello tárfiók **közös hozzáférésű Jogosultságkód beolvasása**.

    ![SAS beszerzése menüpont][13]

2. A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello időtartamot és engedélyeket, hello fiók, és válassza **létrehozása**.

    ![SAS beszerzése párbeszédpanel][14]  
    Hello **közös hozzáférésű Jogosultságkód** párbeszédpanel megnyílik, és hello SAS jeleníti meg.

3. Következő toohello **kapcsolati karakterlánc**, jelölje be **másolási** toocopy azt toohello vágólapra, majd válassza ki **Bezárás**.

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a>Megosztott toohello fiók csatolása hello SAS segítségével
1. A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. A hello **tooAzure tárolási csatlakozás** párbeszédpanelen adja meg a hello kapcsolati karakterláncot, majd válassza ki **következő**.

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy. toomake módosításokat, válassza ki **vissza**, és írja be a kívánt hello beállításokat. 

4. Kattintson a **Csatlakozás** gombra.

5. Miután csatlakozik, hello tárfiók jelenik meg, amely **(SAS)** megadott fióknév toohello lesz hozzáfűzve.

    ![Csatolt tooan fiók SAS használatával eredménye][17]

## <a name="attach-a-service-by-using-an-sas"></a>Szolgáltatás csatolása SAS használatával
hello "Tárfiók csatolása a SAS használatával" szakasz ismerteti, hogyan egy Azure-előfizetés rendszergazdája biztosíthat ideiglenes hozzáférést tooa storage-fiók létrehozása és megosztása hello tárfiók egy SAS-kód. Hasonlóképpen SAS-kód létrehozható adott szolgáltatásokhoz (blob tárolókhoz, üzenetsorokhoz és táblákhoz) is a tárfiókon belül.  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a>Hello szolgáltatást, amelyet az tooshare egy SAS-kód készítése
Ebben a kontextusban a szolgáltatások blob tárolók, üzenetsorok vagy táblák lehetnek. toogenerate hello SAS felsorolt szolgáltatáshoz, lásd:

* [Hello SAS lekérése blob tárolóhoz](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Hello SAS lekérése fájlmegosztás: *hamarosan elérhető*
* Hello SAS lekérése várólista: *hamarosan elérhető*
* Hello SAS lekérése táblához: *hamarosan elérhető*

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a>A szolgáltatás hello SAS toohello közös fiók csatolása
1. A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. A hello **tooAzure tárolási csatlakozás** párbeszédpanelen adja meg a hello SAS URI-t, és válassza **következő**.

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy. toomake módosításokat, válassza ki **vissza**, és írja be a kívánt hello beállításokat. 

4. Kattintson a **Csatlakozás** gombra.

5. Miután csatlakozik, hello újonnan csatolt szolgáltatás alatt jelenik meg hello **(szolgáltatás SAS)** csomópont.

    ![Eredménye tooa megosztott szolgáltatás csatolása a SAS használatával][20]

## <a name="search-for-storage-accounts"></a>Tárfiókok keresése
Ha storage-fiókok listája túl hosszú, egy adott tárfiókok gyorsan toolocate toouse hello keresőmezőbe hello tetején hello bal oldali ablaktáblán.

Hello keresőmezőbe írja be, mert hello bal oldali ablak megjeleníti hello tárfiókokat jeleníti toothat pont be a megadott hello keresett érték. Például egy keresési összes tárfiókot, amelynek a neve tartalmazza **tarcher** megjelenik-e a következő képernyőkép hello:

![Tárfiók keresése][11]

## <a name="next-steps"></a>Következő lépések
* [Azure Blob Storage-erőforrások kezelése a Tártallózó (előzetes verzió) használatával](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
