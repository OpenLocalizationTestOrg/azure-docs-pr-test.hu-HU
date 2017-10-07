---
title: "aaaHow toomanage hello Azure portál Azure File storage |} Microsoft Docs"
description: "Ismerje meg, hogy toouse hello Azure portál toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 385b99ac1c3d97ca79059ae8229afb53f1e825cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a>Hogyan toouse Azure File storage hello Azure portál
Hello [Azure-portálon](https://portal.azure.com) felhasználói felülete lehetőséget nyújt az Azure File storage kezelése. Hello követő műveletek webböngészőből végezheti el:

* Fájlmegosztás létrehozása
* Töltse fel, és a fájlok tooand letöltése a fájlmegosztásból.
* Hello az egyes fájlmegosztások valós használatának figyelése.
* Hello fájl megosztás méretkvótájának módosítása
* Hello csatlakoztatási parancsok toouse toomount a fájlmegosztás másolása a Windows vagy Linux rendszerű ügyfél.

## <a name="create-file-share"></a>Fájlmegosztás létrehozása
1. Bejelentkezés toohello Azure-portálon.
2. Hello navigációs menüjében kattintson **tárfiókok** vagy **tárfiókok (klasszikus)**.
    
    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Válassza ki a tárfiókját.

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Válassza a „Fájlok” szolgáltatást.

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Kattintson a "Fájlmegosztások", és kövesse az első fájlmegosztás hello hivatkozás toocreate.

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Adja meg hello fájlmegosztás neve és hello fájl (felfelé too5120 GB) megosztás toocreate hello méretét az első fájlmegosztását. Ha hello fájlmegosztás létrejött, bármely fájlrendszerből, amely támogatja az SMB 2.1 vagy SMB 3.0 csatlakoztathatja azt. Sikerült kattintva **kvóta** hello megosztás toochange hello fájlméret hello fájl mentése too5120 GB. Tekintse meg a túl[Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/) az Azure File storage tooestimate hello tárolási költsége.

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Fájlok fel- és letöltése
1. Válasszon egy már létrehozott fájlmegosztást.

    ![Hogyan tooupload és a letöltési fájlok hello portal képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Kattintson a **feltöltése** a fájlok feltöltésére hello felhasználói felületének megnyitásához.

    ![Hogyan tooupload hello portal fájljainak képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a>Csatlakozás toofile megosztás
-  Kattintson a **Connect** csatlakoztatását a hello fájlmegosztás hello parancssori lekérése a Windows és Linux. A Linux-felhasználók számára, olvassa el a is túl[hogyan toouse Linux Azure File storage](storage-how-to-use-files-linux.md) más Linux terjesztésekről további csatlakoztatását a útmutatót.

    ![Hogyan toomount hello fájlmegosztás képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  Hello fájl elhelyezésére szolgáló parancsok Windows vagy Linux megosztáshoz, majd futtassa az Ön Azure virtuális gép vagy a helyi gép másolhatja.

    ![Képernyőkép a hello csatlakoztatási parancsok Windows és Linux rendszerekhez](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tipp:**  
toofind hello tárfiók_elérési_kulcsa csatlakoztatáshoz, kattintson a **nézet elérési kulcsainak a tárfiókhoz** hello hello alján csatlakozzon a lap.

## <a name="see-also"></a>Lásd még:
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

* [Gyakori kérdések](storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-file-connection-problems.md)