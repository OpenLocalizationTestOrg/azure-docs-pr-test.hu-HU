---
title: "Kapcsolódó szolgáltatások a Visual Studio használatával Azure Storage aaaAdd |} Microsoft Docs"
description: "A Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen hello Azure Storage tooyour alkalmazás hozzáadása"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Az Azure storage hozzáadása a Visual Studio kapcsolódó szolgáltatások használatával
A Visual Studio kapcsolódhatnak tooAzure tárolási hello segítségével a következő hello bármelyikét **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel:

- C#-felhőszolgáltatás
- .NET-háttérrendszer mobilszolgáltatás
- ASP.NET-webhely vagy szolgáltatás
- Az ASP.NET Core szolgáltatás
- Azure webjobs-feladat szolgáltatás 

hello csatlakoztatott működtetéséhez szükséges hello hivatkozások és a kapcsolat kód tooyour projekt hozzáadása, és megfelelően módosítja a konfigurációs fájlok. 

A művelet befejezését követően hello **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel automatikusan dokumentációját, és részletesen leírja a hello lépéseket szükséges toostart blob-tároló, üzenetsorok és táblák használata jeleníti meg.

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a>Csatlakozás tooAzure tárolóhely-hello kapcsolódó szolgáltatások párbeszédpanel
1. Nyissa meg a projektet a Visual Studio

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **kapcsolódó szolgáltatások** csomópont, és hello helyi menüt, majd válassza a **kapcsolódó szolgáltatás hozzáadása**.
   
    ![Adja hozzá az Azure szolgáltatás csatlakoztatva](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. A hello **kapcsolódó szolgáltatások** lapon jelölje be **felhőalapú tárolás az Azure Storage**.
   
    ![Az Azure Storage hozzáadása](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. A hello **Azure Storage** párbeszédpanel, válasszon egy meglévő tárfiókot használ, válassza ki **Hozzáadás**.
   
    Ha toocreate a storage-fiókra van szüksége, lépjen a toohello következő lépésre. Egyéb esetben folytassa a toostep 6.
    
    ![Adja hozzá a meglévő tárolási fiók tooproject](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. a tárfiók toocreate: 
   
   1. Válassza ki **hozzon létre egy új Tárfiókot** hello hello párbeszédpanel alsó részén.

   1. Töltse ki a hello **Storage-fiók létrehozása** párbeszédpanel, válassza ki **létrehozása**.
      
       ![Új Azure storage-fiók](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Ha hello **Azure Storage** párbeszédpanel jelenik meg, az új tárfiók hello hello listájában jelenik meg. Új tárfiók hello hello listában válassza ki és **Hozzáadás**.

1. csatlakoztatott szolgáltatás hello alatt jelenik meg tárolási hello **Szolgáltatáshivatkozások** a projekt csomópontjára.
   
## <a name="how-your-project-is-modified"></a>A projekt módosítása hogyan
Hello párbeszédpanel befejezése után a Visual Studio hivatkozásokat ad, és módosítja az egyes konfigurációs fájlok. hello adott módosítások hello projekt típusától függ: 

- ASP.NET-projekt - [Mi történt – ASP.NET-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- Az ASP.NET Core projekt - [Mi történt – ASP.NET 5 projektek](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Felhőszolgáltatás-projekt (webes és feldolgozói szerepkörök) - [Mi történt – Cloud Service projektek](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Webjobs-feladat projekt - [Mi történt – a webjobs-feladat projektek](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Következő lépések
- [MSDN fórum: Az Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [A Microsoft Azure tárolás fejlesztői Blog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Az Azure Storage-dokumentáció](https://docs.microsoft.com/azure/storage/)
