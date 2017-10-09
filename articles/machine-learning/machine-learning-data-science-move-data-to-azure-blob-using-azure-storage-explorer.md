---
title: "a Azure Tártallózó blobtárolóból aaaMove adatok tooand |} Microsoft Docs"
description: "Adatok tooand áthelyezése az Azure Blob Storage Azure Storage Explorerrel"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 38d3bc009950c97d8474b0acceaf74814638dac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azure-storage-explorer"></a>Adatok tooand áthelyezése az Azure Blob Storage Azure Storage Explorerrel
Az Azure Tártallózó egy szabad eszköz, amely lehetővé teszi az Azure Storage-adatokkal Windows, a macOS és a Linux toowork Microsoft. Ez a témakör ismerteti, hogyan toouse azt tooupload és a letöltési adatokat az Azure blob-tároló. hello eszköz letölthető a [Microsoft Azure Tártallózó](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Ha virtuális Gépet, amely hello parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), majd Azure Tártallózó hello VM már telepítve van.
> 
> [!NOTE]
> A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és [Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és hello megfelelő kulcs fiók rendelkezik. Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs. 

* tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md). Csak a kulcsfontosságú tooconnect toohello fiók hello Azure Tártallózó eszközzel szeretne, ellenőrizze a Megjegyzés hello hozzáférési kulcsot a tárfiók.
* hello Azure Tártallózó eszköz letölthető a [Microsoft Azure Tártallózó](http://storageexplorer.com/). Fogadja el a hello alapértelmezett során telepíti.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Az Azure Storage-kezelővel
a következő lépéseket a dokumentum hogyan hello Azure Tártallózó tooupload/letöltés adatokat. 

1. Indítsa el a Microsoft Azure Tártallózó.
2. hello mentése toobring **jelentkezzen be fiókjával tooyour...**  varázslóban válassza **Azure-fiók beállításai** ikonra, majd **vegyen fel egy fiókot** és adja meg hitelesítő adatokat. ![Egy Azure storage-fiók hozzáadása](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. hello mentése toobring **tooAzure tárolási csatlakozás** varázsló, jelölje be hello **csatlakoztassa tooAzure tárterületet** ikon. ![Csatlakoztassa a tárterületet tooAzure](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Az Azure storage-fiók hozzáférési kulcs hello meg hello **tooAzure tárolási csatlakozás** varázsló, majd **következő**. ![Csatlakoztassa a tárterületet tooAzure](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Adja meg a tárfiók neve hello **fióknév** mezőbe, majd válassza ki **következő**. ![Külső tárterület csatolása](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. most már hozzáadott hello tárfiók szerepelnie kell. toocreate egy blob tároló tárfiókokban, kattintson a jobb gombbal a hello **Blobtárolók** fiók, jelölje be a csomópont **Blob-tároló létrehozása**, és adjon meg egy nevet.
7. tooupload adatok tooa tároló, jelölje be hello cél konténert, és kattintson hello **feltöltése** gomb.![ Storage-fiókok](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Kattintson a hello **...**  sarkában hello toohello **fájlok** mezőben, jelöljön ki egy vagy több fájlok tooupload hello fájlrendszer majd **feltöltése** hello fájlok feltöltése toobegin.![ Fájlok feltöltése](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. toodownload adatok hello kiválasztásával hello megfelelő tárolót toodownload a blob, és kattintson a **letöltése**. ![Fájlok letöltése](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

