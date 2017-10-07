---
title: "az Azure file storage egy Windows Azure virtuális gépről aaaMount |} Microsoft Docs"
description: "Az Azure file storage szolgáltatással hello felhőben tárolja a fájlt, és a felhőalapú fájlmegosztást csatlakoztathatja egy Azure virtuális gép (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure fájlmegosztásokat Windows virtuális gépek használata 

Is használhatja az Azure fájlmegosztások egy módon toostore és hozzáférni a fájlokhoz a virtuális gépről. Tárolhatja például a parancsfájl vagy az, hogy szeretné-e a virtuális gépek tooshare konfigurációs fájlját. Ebben a témakörben megmutatjuk, hogyan toocreate és a csatlakoztatási egy Azure-fájlmegosztás, és hogyan tooupload és a letöltési fájlokat.

## <a name="connect-tooa-file-share-from-a-vm"></a>Tooa fájlmegosztás csatlakoztatja a virtuális gép

Ez a szakasz azt feltételezi, hogy már rendelkezik, amelyet a tooconnect fájlmegosztással. Ha egy toocreate van szüksége, tekintse meg [fájlmegosztás létrehozása](#create-a-file-share) a témakör későbbi részében.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello bal oldali menüben kattintson **tárfiókok**.
3. Válassza ki a tárfiókját.
4. A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.
5. Válasszon egy fájlmegosztást.
6. Kattintson a **Connect** tooopen egy oldal, amely hello parancssori szintaxist hello fájlmegosztás csatlakoztatása a Windows vagy Linux jeleníti meg.
7. Jelöljön ki hello hello parancs szintaxisát, és illessze be a Jegyzettömb vagy valahol máshol ahol könnyedén elérheti azt. 
8. Hello szintaxis tooremove hello bevezető szerkesztése ** > **, és cserélje le *[meghajtó_betűjele]* hello meghajtóbetűjellel (például **y**) toomount hello fájlmegosztás helyének.
8. Csatlakozás a virtuális gép tooyour, és nyisson meg egy parancssort.
9. Illessze be hello kapcsolat szintaxis szerkeszteni, majd nyomja le **Enter**.
10. Ha hello kapcsolat létrejött, a hibaüzenet hello **hello parancs sikeresen befejeződött.**
11. Ellenőrizze a hello kapcsolatot írja be a hello meghajtó betűjele tooswitch toothat meghajtó, és írja be **dir** hello fájlmegosztás toosee hello tartalmát.



## <a name="create-a-file-share"></a>Fájlmegosztás létrehozása 
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello bal oldali menüben kattintson **tárfiókok**.
3. Válassza ki a tárfiókját.
4. A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.
5. Hello Fájlszolgáltatás oldalon kattintson **+ fájlmegosztás** az első fájlmegosztás toocreate. \
6. Töltse ki hello fájlmegosztás neve. A fájlmegosztás neve kisbetűket, számokat és kötőjeleket egyetlen használhatja. hello neve nem kezdődhet kötőjellel, és nem használható több egymást követő kötőjelet. 
7. Adja meg a maximális méretét hello fájl mentése too5120 GB lehet.
8. Kattintson a **OK** toodeploy hello fájlmegosztást.
   
## <a name="upload-files"></a>Fájlok feltöltése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello bal oldali menüben kattintson **tárfiókok**.
3. Válassza ki a tárfiókját.
4. A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.
5. Válasszon egy fájlmegosztást.
6. Kattintson a **feltöltése** tooopen hello **fájlok feltöltése** lap.
7. Kattintson a hello mappa ikon toobrowse egy fájl tooupload a helyi fájlrendszerben.   
8. Kattintson a **feltöltése** tooupload hello fájl toohello fájlmegosztást.

## <a name="download-files"></a>Fájlok letöltése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello bal oldali menüben kattintson **tárfiókok**.
3. Válassza ki a tárfiókját.
4. A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.
5. Válasszon egy fájlmegosztást.
6. Kattintson a jobb gombbal a hello fájlt, és válassza a **letöltése** toodownload azt tooyour helyi számítógép.
   

## <a name="next-steps"></a>Következő lépések

Is létrehozása és kezelése a PowerShell használatával fájlmegosztások. További információkért lásd: [Ismerkedés az Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
