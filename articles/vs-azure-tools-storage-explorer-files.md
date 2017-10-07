---
title: "a Tártallózó (előzetes verzió) az Azure File storage aaaUsing |} Microsoft Docs"
description: "Megtudhatja, hogyan megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork fájllal közösen használja, és fájlokat."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>A Storage Explorer (előzetes verzió) használata az Azure File Storage szolgáltatással

Az Azure File storage egy olyan szolgáltatás, amely kínál a fájl osztja meg a hello felhő használatával hello szabványos Server Message Block (SMB) protokollt. Az SMB 2.1 és az SMB 3.0 protokollt is támogatja. Az Azure File storage szolgáltatással telepítheti át gyorsan és költséges újraírások nélkül fájl megosztások tooAzure támaszkodó örökölt alkalmazások. Használhatja a fájladatok tárolási tooexpose nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak. Ebből a cikkből megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork fájllal közösen használja, és fájlokat.

## <a name="prerequisites"></a>Előfeltételek

toocomplete hello cikkben leírt lépéseket, az alábbi hello lesz szüksége:

- [A Tártallózó (előzetes verzió) letöltése és telepítése](http://www.storageexplorer.com/)

- [Tooa Azure storage-fiók vagy a szolgáltatás](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Fájlmegosztás létrehozása

Minden fájlnak fájlmegosztásban kell lennie, amely egyszerűen egy logikai fájlcsoportot jelent. Egy fiók korlátlan számú fájlmegosztást tartalmazhat, egy adott megosztás pedig korlátlan számú fájl tárolására használható.

hello lépések bemutatják, hogyan toocreate fájlmegosztás belül a Tártallózó (előzetes verzió).

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblán bontsa ki a hello tárfiókot, amelyen belül kívánja toocreate hello fájlmegosztás

3. Kattintson a jobb gombbal **fájlmegosztások**, és válassza – hello helyi menüből – a **fájlmegosztás létrehozása**.

    ![Fájlmegosztás létrehozása](media/vs-azure-tools-storage-explorer-files/image1.png)

4. A szövegmezőben megjelenik, hello alatt **fájlmegosztások** mappát. Adja meg a fájlmegosztás hello nevét. Lásd: hello [elnevezési szabályok megosztása](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) szakasz listáját a szabályok és a fájlmegosztásokat naming korlátozásai.

    ![Elnevezési hello megosztás](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Nyomja le az **Enter** kész toocreate hello fájlmegosztáshoz, amikor vagy **Esc** toocancel. Hello fájlmegosztás sikeres létrehozását követően megjelenik a hello **fájlmegosztások** hello mappa kiválasztott tárfiók.

    ![hello új megosztás](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Fájlmegosztás tartalmának megtekintése

A fájlmegosztások fájlokat és mappákat tartalmaznak (a mappákban pedig további fájlok lehetnek).

hello következő lépések bemutatják, hogyan fájl tartalmának tooview hello megosztása belül a Tártallózó (előzetes verzió): +

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblán bontsa ki a tooview kívánja hello fájlmegosztás tartalmazó hello tárfiókot.

3. Bontsa ki a hello tárfiók **fájlmegosztások**.

4. Kattintson a jobb gombbal hello fájlmegosztás meg akarja tooview, és válassza – a hello helyi menüből – **nyitott**. Kattintson duplán a tooview kívánja hello fájlmegosztás is.

    ![Megosztás megnyitása](media/vs-azure-tools-storage-explorer-files/image4.png)

5. hello fő ablaktábla hello fájl megosztott tartalmat.
    
    ![hello a megosztás fájlkiszolgálója tartalma](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Fájlmegosztás törlése

A fájlmegosztások szükség szerint egyszerűen létrehozhatók és törölhetők. (toosee hogyan toodelete fájlokat, tekintse meg a toohello szakasz [fájljait egy fájlmegosztásban kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

hello lépések bemutatják, hogyan toodelete fájlmegosztás belül a Tártallózó (előzetes verzió):

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblán bontsa ki a tooview kívánja hello fájlmegosztás tartalmazó hello tárfiókot.

3. Bontsa ki a hello tárfiók **fájlmegosztások**.

4. Kattintson a jobb gombbal hello fájlmegosztás meg akarja toodelete, és válassza – a hello helyi menüből – **törlése**. Is **törlése** toodelete hello kijelölt fájlmegosztást.

    ![Törlés](media/vs-azure-tools-storage-explorer-files/image6.png)

5. Válassza ki **Igen** toohello megerősítő párbeszédpanele.
    
    ![Megerősítési párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Fájlmegosztás másolása

A Tártallózó (előzetes verzió) lehetővé teszi egy fájl megosztási toohello vágólapra toocopy, és illessze be egy másik tárfiókhoz fájlmegosztás. (toosee hogyan toocopy fájlokat, tekintse meg a toohello szakasz [fájljait egy fájlmegosztásban kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

hello lépések bemutatják, hogyan toocopy egy fájlt egy tárolási fiók tooanother megoszthatja.

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblán bontsa ki a toocopy kívánja hello fájlmegosztás tartalmazó hello tárfiókot.

3. Bontsa ki a hello tárfiók **fájlmegosztások**.

4. Kattintson a jobb gombbal hello fájlmegosztás meg akarja toocopy, és válassza – a hello helyi menüből – **másolási fájlmegosztás**.

    ![Fájlmegosztás másolása](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Kattintson a jobb gombbal a kívánt hello "target" tárfiók, amelybe azt szeretné, hogy toopaste hello fájlmegosztás, és válassza – a hello helyi menüből – **Beillesztés fájlmegosztás**.

    ![Fájlmegosztás beillesztése](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a>Hello SAS lekérése fájlmegosztás

A [közös hozzáférésű jogosultságkód (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) delegált hozzáférést tooresources a tárfiókban lévő biztosít. Ez azt jelenti, hogy egy ügyfél csak korlátozott engedélyekkel tooobjects a tárfiókban lévő egy megadott időszakban, és engedélyeket, megadott számú anélkül, hogy tooshare a tárelérési kulcsok biztosíthat.

hello következő lépések bemutatják, hogyan toocreate egy SAS-t egy fájl megosztása: +

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblájában bontsa ki, amelynek kívánja tooget SAS hello fájlmegosztás tartalmazó hello tárfiókot.

3. Bontsa ki a hello tárfiók **fájlmegosztások**.

4. Kattintson a jobb gombbal a kívánt fájlmegosztás hello, és válassza – a hello helyi menüből – **közös hozzáférésű Jogosultságkód beolvasása**.

    ![Közös hozzáférésű jogosultságkód igénylése](media/vs-azure-tools-storage-explorer-files/image10.png)

5. A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello házirend, érvényességének kezdő és záró dátumát, időzóna, és a hozzáférési szintet hello erőforrás használni szeretne.

    ![SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image11.png)

6. Ha befejezte a hello SAS-beállítások, válassza ki a **létrehozása**.

7. Egy második **közös hozzáférésű Jogosultságkód** párbeszédpanel ezután jeleníti meg, hogy listák hello fájlmegosztás hello URL-cím mellett, és használhatja a tooaccess QueryStrings hello tárolási erőforrás. Válassza ki **másolási** toocopy toohello vágólapra kívánja tovább toohello URL.
    
    ![Második SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image12.png)

8. Ha elkészült, válassza a **Bezárás** lehetőséget.

## <a name="manage-access-policies-for-a-file-share"></a>Fájlmegosztás hozzáférési szabályzatainak kezelése

hello következő lépések bemutatják, hogyan toomanage (hozzáadása és eltávolítása) hozzáférési házirendek egy fájlmegosztás: +. hello hozzáférési házirendek SAS URL-címek, amelyek személy tooaccess hello tárolófájl erőforrás használhatja egy meghatározott időszakban létrehozására szolgál.

1. Nyissa meg a Tártallózót (előzetes verzió).

2. Hello bal oldali ablaktáblán bontsa ki a hello fájlmegosztást, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.

3. Bontsa ki a hello tárfiók **fájlmegosztások**.

4. Válassza ki a kívánt fájlmegosztás hello, és válassza – a hello helyi menüből – **hozzáférési házirendek kezelése**.

    ![Hozzáférési szabályzatok kezelése helyi menü](media/vs-azure-tools-storage-explorer-files/image13.png)

5. Hello **hozzáférési házirendek** párbeszédpanel megjelennek a kijelölt hello fájlmegosztás már létrehozott hozzáférési házirendekben.
    
    ![Hozzáférési szabályzatok](media/vs-azure-tools-storage-explorer-files/image14.png)

6. Kövesse az alábbi lépéseket attól függően, hogy hello hozzáférési házirend fájlkezelési feladat:
    
    - **Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget. Létrehozott, miután hello **hozzáférési házirendek** párbeszédpanel megjeleníti az újonnan hozzáadott hello házirendhez (az alapértelmezett beállításokkal).

    - **Hozzáférési szabályzat szerkesztése** – Hajtsa végre a kívánt módosításokat, majd válassza a **Mentés** lehetőséget.

    - **Távolítsa el a hozzáférési házirendek** – Itt adhatja meg **eltávolítása** tooremove kívánja tovább toohello házirend.

7. Hozzon létre egy új SAS URL-CÍMÉT a korábban létrehozott házirend hello:
    
    ![SAS beszerzése](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Az SAS neve és tulajdonságai](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Fájlmegosztásban lévő fájlok kezelése

Ha létrehozta a fájlmegosztást, fájlmegosztás toothat fájl feltöltéséhez, töltse le a fájl tooyour helyi számítógépen, nyissa meg a fájlt a helyi számítógépen, és még sok más.

hello következő lépések bemutatják, hogyan toomanage hello fájlokat (és mappákat) a fájlon belüli megosztani.

1.  Nyissa meg a Tártallózót (előzetes verzió).

2.  Hello bal oldali ablaktáblán bontsa ki a toomanage kívánja hello fájlmegosztás tartalmazó hello tárfiókot.

3.  Bontsa ki a hello tárfiók **fájlmegosztások**.

4.  Kattintson duplán a tooview kívánja hello fájlmegosztást.

5.  hello fő ablaktábla hello fájl megosztott tartalmat.

    ![hello a megosztás fájlkiszolgálója tartalma](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  hello fő ablaktábla hello fájl megosztott tartalmat.

7.  Kövesse az alábbi lépéseket attól függően, hello feladat kívánja tooperform:

    - **Fájlok tooa fájlmegosztás feltöltése**

        a.  Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **fájl feltöltése** hello legördülő menüből.

        ![Fájlok feltöltése](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **fájlok** szövegmezőben tooselect hello (oka) t tooupload kívánja.

        ![Fájlok hozzáadása](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. Válassza a **Feltöltés** lehetőséget.

    - **A mappa tooa fájlmegosztás feltöltése**
        
        a. Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **feltöltése mappa** hello legördülő menüből.

        ![Mappa feltöltése menü](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. A hello **feltöltési mappa** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **mappa** szöveg mezőben tooselect hello mappa tartalma tooupload kívánja.

        c. Szükség esetén adja meg, hogy a célmappa, mely hello a kijelölt mappa tartalma lesz feltöltve. Ha hello célmappa nem létezik, a rendszer létrehozza.

        d. Válassza a **Feltöltés** lehetőséget.

    - **Töltse le a fájl tooyour helyi számítógépről**
        
        a. Válassza ki a toodownload kívánja hello fájlt.
        
        b. Hello fő ablaktáblán eszköztáron válassza **letöltése**.
        
        c. A hello **adja meg, ahol toosave hello letöltött fájl** párbeszédpanelen adja meg a hello helyének letöltött hello fájlt, és hello neve toogive kívánja azt.

        d. Kattintson a **Mentés** gombra.

    - **Fájl megnyitása a helyi számítógépen**
        
        a.  Válassza ki a tooopen kívánja hello fájlt.
        
        b.  Hello fő ablaktáblán eszköztáron válassza **nyitott**.
        
        c.  hello fájl tölthető le, és hello fájl fájl alaptípusának társított hello alkalmazás használatával megnyitni.

    - **A fájl toohello vágólapra másolása**

        a. Válassza ki a toocopy kívánja hello fájlt.

        b. Hello fő ablaktáblán eszköztáron válassza **másolási**.

        c. Hello bal oldali ablaktáblán, keresse meg a fájlmegosztás tooanother, és kattintson rá duplán tooview azt hello fő ablaktáblán.

        d. Hello fő ablaktáblán eszköztáron válassza **Beillesztés** toocreate hello fájl egy példányát.

    - **Fájl törlése**

        a. Válassza ki a toodelete kívánja hello fájlt.

        b. Hello fő ablaktáblán eszköztáron válassza **törlése**.

        c. Válassza ki **Igen** toohello megerősítő párbeszédpanele.

## <a name="next-steps"></a>Következő lépések

- Nézet hello [legújabb Tártallózó (előzetes verzió) kibocsátási megjegyzések és videók](http://www.storageexplorer.com/).

- Ismerje meg, hogyan túl[létrehozása az Azure BLOB, táblák, üzenetsorok és fájlokat használó alkalmazások](https://azure.microsoft.com/documentation/services/storage/).
