---
title: "aaaManage Azure Blob Storage-erőforrások a Tártallózó (előzetes verzió) |} Microsoft Docs"
description: "Az Azure Blob-tárolók és Blobok a Tártallózó (előzetes verzió) kezelése"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>A Tártallózó (előzetes verzió) Azure Blob Storage-erőforrások kezelése
## <a name="overview"></a>Áttekintés
[Az Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.
Használhatja a Blob storage tooexpose adatok nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak. Ebből a cikkből megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork a blob-tárolók és blobok.

## <a name="prerequisites"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, az alábbi hello lesz szüksége:

* [A Tártallózó (előzetes verzió) letöltése és telepítése](http://www.storageexplorer.com)
* [Tooa Azure storage-fiók vagy a szolgáltatás](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>A blob-tároló létrehozása
Minden BLOB egy blob tároló, amely egyszerűen blobok logikai csoportosítása kell lennie. Egy fiók korlátlan számú tárolót tartalmazhat, és minden egyes tároló korlátlan számú BLOB tárolhatja.

hello következő lépések bemutatják, hogyan toocreate egy blob tároló a Tártallózó (előzetes verzió) belül.

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello tárfiókot, amelyen belül kívánja toocreate hello blob tároló.
3. Kattintson a jobb gombbal **Blobtárolók**, és válassza – hello helyi menüből – a **Blob-tároló létrehozása**.

   ![Blob tárolók a helyi menü létrehozása][0]
4. A szövegmezőben megjelenik, hello alatt **Blobtárolók** mappa. Adja meg a blob-tároló hello nevét. Lásd: hello [tároló elnevezési szabályait](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) listáját szakasza szabályok és a blob tárolók elnevezési korlátozás.

   ![Szövegmező Blob tárolók létrehozása][1]
5. Nyomja le az **Enter** befejezése után toocreate hello blob tároló, vagy **Esc** toocancel. Ha hello blob tároló sikeresen létrejött, akkor jelenik meg a hello **Blobtárolók** hello mappa kiválasztott tárfiók.

   ![BLOB-tároló létrehozása][2]

## <a name="view-a-blob-containers-contents"></a>A blob-tároló tartalmának megtekintése
BLOB tárolók blobok és mappák (BLOB is tartalmazhat) tartalmazhat.

hello következő lépések bemutatják, hogyan tooview hello tartalmát egy blob-tároló belül a Tártallózó (előzetes verzió):

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló tooview kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Kattintson a jobb gombbal hello blob tároló meg akarja tooview, és válassza – a hello helyi menüből – **Blob tároló szerkesztő megnyitása**.
   Kattintson duplán a hello blob tároló tooview kívánja is.

   ![Nyissa meg a blob tároló szerkesztő helyi menü][19]
5. hello fő ablaktábla hello blob tároló tartalmának.

   ![A BLOB-tároló szerkesztő][3]

## <a name="delete-a-blob-container"></a>A blob-tároló törlése
BLOB tárolók egyszerűen hozható létre és igény szerint törölve. (toosee hogyan toodelete személy blobok, tekintse meg a toohello szakasz [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)

hello következő lépések bemutatják, hogyan toodelete egy blob-tároló belül a Tártallózó (előzetes verzió):

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló tooview kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Kattintson a jobb gombbal hello blob tároló meg akarja toodelete, és válassza – a hello helyi menüből – **törlése**.
   Is **törlése** toodelete hello kijelölt blob tároló.

   ![A blob tároló helyi menü törlése][4]
5. Válassza ki **Igen** toohello megerősítő párbeszédpanele.

   ![A blob tároló megerősítő törlése][5]

## <a name="copy-a-blob-container"></a>Másolja a blob-tároló
A Tártallózó (előzetes verzió) lehetővé teszi a blob-tároló toohello vágólapra toocopy, és illessze be egy másik tárfiókhoz BLOB-tároló. (toosee hogyan toocopy személy blobok, tekintse meg a toohello szakasz [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)

hello lépések bemutatják, hogyan toocopy egy blob tárolót egy tárolási fiók tooanother.

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló toocopy kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Kattintson a jobb gombbal hello blob tároló meg akarja toocopy, és válassza – a hello helyi menüből – **másolási Blob tároló**.

   ![Másolja a blob tároló helyi menü][6]
5. Kattintson a jobb gombbal a kívánt hello "target" tárfiók, amelybe azt szeretné, hogy toopaste hello blob tároló, és válassza – a hello helyi menüből – **Beillesztés Blob tároló**.

   ![Beillesztés blob tároló helyi menü][7]

## <a name="get-hello-sas-for-a-blob-container"></a>Hello SAS lekérése blob tárolóhoz
A [közös hozzáférésű jogosultságkód (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) delegált hozzáférést tooresources a tárfiókban lévő biztosít.
Ez azt jelenti, hogy egy ügyfél csak korlátozott engedélyekkel tooobjects a tárfiókban lévő egy megadott időszakban, és engedélyeket, megadott számú anélkül, hogy a fiók hozzáférési kulcsait megosztásához biztosíthat.

hello következő lépések bemutatják, hogyan toocreate egy SAS-t egy blob-tároló:

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek kívánja tooget Aláírást tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Kattintson a jobb gombbal a hello kívánt blob tároló, és válassza – a hello helyi menüből – **közös hozzáférésű Jogosultságkód beolvasása**.

   ![A helyi menü SAS lekérése][8]
5. A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello házirend, érvényességének kezdő és záró dátumát, időzóna, és a hozzáférési szintet hello erőforrás használni szeretne.

   ![SAS-beállítások beolvasása][9]
6. Ha befejezte a hello SAS-beállítások, válassza ki a **létrehozása**.
7. Egy második **közös hozzáférésű Jogosultságkód** párbeszédpanel ezután jeleníti meg, hogy listák hello blob tároló hello URL-cím mellett, és használhatja a tooaccess QueryStrings hello tárolási erőforrás.
   Válassza ki **másolási** toocopy toohello vágólapra kívánja tovább toohello URL.

   ![SAS URL-címének másolása][10]
8. Ha elkészült, válassza a **Bezárás** lehetőséget.

## <a name="manage-access-policies-for-a-blob-container"></a>Egy blob tároló a hozzáférési házirendek kezelése
hello következő lépések bemutatják, hogyan toomanage (hozzáadása és eltávolítása) hozzáférési házirendek egy blob-tároló:

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Válassza ki a kívánt blobtárolóban hello, és válassza – a hello helyi menüből – **hozzáférési házirendek kezelése**.

   ![Hozzáférési szabályzatok kezelése helyi menü][11]
5. Hello **hozzáférési házirendek** párbeszédpanel felsorolja hello kijelölt blob tároló már létrehozott hozzáférési házirendekben.

   ![Hozzáférési házirend beállítása][12]        
6. Kövesse az alábbi lépéseket attól függően, hogy hello hozzáférési házirend fájlkezelési feladat:

   * **Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget. Létrehozott, miután hello **hozzáférési házirendek** párbeszédpanel megjeleníti az újonnan hozzáadott hello házirendhez (az alapértelmezett beállításokkal).
   * **A hozzáférési házirendek szerkesztése** – végezze el a kívánt módosításokat, és válassza ki **mentése**.
   * **Távolítsa el a hozzáférési házirendek** – Itt adhatja meg **eltávolítása** tooremove kívánja tovább toohello házirend.

## <a name="set-hello-public-access-level-for-a-blob-container"></a>A blob-tároló hello nyilvános hozzáférési szint beállítása
Alapértelmezés szerint minden blob tároló túl értéke "Nem nyilvános hozzáférés".

hello következő lépések bemutatják, hogyan toospecify nyilvános hozzáférési szint egy blob-tároló.

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Válassza ki a kívánt blobtárolóban hello, és válassza – a hello helyi menüből – **nyilvános hozzáférési szint beállítása**.

   ![Állítsa be a nyilvános hozzáférés szint a helyi menü][13]
5. A hello **tároló nyilvános hozzáférési szint beállítása** párbeszédpanelen adja meg a szükséges hello hozzáférési szintet.

   ![Nyilvános hozzáférés szint beállításainak megadása][14]
6. Kattintson az **Alkalmaz** gombra.

## <a name="managing-blobs-in-a-blob-container"></a>A blob-tárolóban lévő blobok kezelése
A blob-tároló létrehozását követően feltöltése a blob toothat blobtárolóban, töltse le a blob tooyour helyi számítógépen, nyissa meg a blob a helyi számítógépen, és még sok más.

hello lépések bemutatják, hogyan toomanage hello blobok (és mappákat) a blob-tárolóban.

1. Nyissa meg a Tártallózót (előzetes verzió).
2. Hello bal oldali ablaktáblán bontsa ki a hello blob tároló toomanage kívánja tartalmazó hello tárfiókot.
3. Bontsa ki a hello tárfiók **Blobtárolók**.
4. Kattintson duplán a hello blob tároló tooview kívánja.
5. hello fő ablaktábla hello blob tároló tartalmának.

   ![Nézet blob tároló][3]
6. hello fő ablaktábla hello blob tároló tartalmának.
7. Kövesse az alábbi lépéseket attól függően, hello feladat kívánja tooperform:

   * **Fájlok tooa blob tároló feltöltése**

     1. Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **fájl feltöltése** hello legördülő menüből.

        ![Fájl menü feltöltése][15]
     2. A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **fájlok** szövegmezőben tooselect hello (oka) t tooupload kívánja.

        ![Töltse fel a fájlok beállítások][16]
     3. Adja meg a hello típusú **Blob-típusú**. hello cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello hello különbségei különféle blob ismerteti.
     4. Szükség esetén adjon meg egy cél mappát, amelybe hello kijelölt fájlok lesz feltöltve. Ha hello célmappa nem létezik, a rendszer létrehozza.
     5. Válassza a **Feltöltés** lehetőséget.
   * **A mappa tooa blobtárolóban feltöltése**

     1. Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **feltöltése mappa** hello legördülő menüből.

        ![Mappa feltöltése menü][17]
     2. A hello **feltöltési mappa** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **mappa** szöveg mezőben tooselect hello mappa tartalma tooupload kívánja.

        ![Töltse fel a mappa beállításai][18]
     3. Adja meg a hello típusú **Blob-típusú**. hello cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello hello különbségei különféle blob ismerteti.
     4. Szükség esetén adja meg, hogy a célmappa, mely hello a kijelölt mappa tartalma lesz feltöltve. Ha hello célmappa nem létezik, a rendszer létrehozza.
     5. Válassza a **Feltöltés** lehetőséget.
   * **Töltse le a blob tooyour helyi számítógépről**

     1. Válassza ki a hello blob toodownload kívánja.
     2. Hello fő ablaktáblán eszköztáron válassza **letöltése**.
     3. A hello **adja meg, ahol toosave hello letöltött blob** párbeszédpanelen adja meg a letöltött hello blob kívánt hello helyre, és hello toogive kívánja neve azt.  
     4. Kattintson a **Mentés** gombra.
   * **Nyissa meg a helyi számítógépen blob**

     1. Válassza ki a hello blob tooopen kívánja.
     2. Hello fő ablaktáblán eszköztáron válassza **nyitott**.
     3. hello blob le lesznek töltve, és hello blob fájl alaptípusának társított hello alkalmazás használatával megnyitni.
   * **A blob toohello vágólapra másolása**

     1. Válassza ki a hello blob toocopy kívánja.
     2. Hello fő ablaktáblán eszköztáron válassza **másolási**.
     3. Hello bal oldali ablaktáblán, keresse meg a tooanother blob tároló, és kattintson rá duplán tooview azt hello fő ablaktáblán.
     4. Hello fő ablaktáblán eszköztáron válassza **Beillesztés** toocreate hello blob egy példányát.
   * **Blobok törléséhez**

     1. Válassza ki a hello blob toodelete kívánja.
     2. Hello fő ablaktáblán eszköztáron válassza **törlése**.
     3. Válassza ki **Igen** toohello megerősítő párbeszédpanele.

## <a name="next-steps"></a>Következő lépések
* Nézet hello [legújabb Tártallózó (előzetes verzió) kibocsátási megjegyzések és videók](http://www.storageexplorer.com).
* Ismerje meg, hogyan túl[létrehozása az Azure BLOB, táblák, üzenetsorok és fájlokat használó alkalmazások](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
