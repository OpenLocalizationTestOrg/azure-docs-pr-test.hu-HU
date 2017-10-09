---
title: "a Visual Studio Azure felhők titkos aaaAccessing |} Microsoft Docs"
description: "Ismerje meg, hogyan tooaccess magánfelhő erőforrások Visual Studio használatával."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>A Visual Studio Azure magánfelhőkben elérése
Alapértelmezés szerint a Visual Studio támogatja a nyilvános Azure-felhőbe REST-végpontok. Ebben a témakörben elsajátíthatja, hogyan toouse a magánfelhő tartozó tanúsítvány tooaccess - és kommunikálhat - hello magánfelhő a Visual Studio eszközből.

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a>a Visual Studio cloud tooaccess titkos Azure
1. A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello magánfelhő, töltse le a közzétételi-beállítások fájlt, vagy forduljon a rendszergazdához közzététele beállításfájl. Azure verzióján nyilvános hello, ez a hivatkozás toodownload hello [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (hello letöltött fájl kiterjesztése kell `.publishsettings`)

1. Nyissa meg a Visual Studio

1. A **Server Explorer**, kattintson a jobb gombbal hello **Azure** csomópont és hello helyi menüből válassza ki a **kezelése és a szűrő előfizetések**.
   
    ![Előfizetések parancs kezelése](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. A hello **kezelése a Microsoft Azure-előfizetések** párbeszédpanelen jelölje be hello **tanúsítványok** lapra, majd válassza ki **importálási**.
   
    ![Az Azure tanúsítványok importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. A hello **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **Tallózás**.

    ![Tallózás gomb hello importálása a Microsoft Azure-előfizetések párbeszédpanelen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. A hello **nyitott** párbeszédpanelen Tallózás toohello könyvtárába, ahol Ön mentett hello közzétételi beállítási fájl, jelölje be hello fájl, és adja **nyitott**.

    ![Hello közzétételi beállítási fájl kiválasztása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Amikor toohello visszaadott **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **importálási**.

    ![Hello közzétételi beállítási fájl importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    hello tanúsítványok hello közzétételi beállítások fájlját a rendszer importálja a parancsmagmodulokat a Visual Studio, és most kezelheti a magánfelhő-alapú erőforrásokhoz.
   
## <a name="next-steps"></a>Következő lépések
- [Közzétételi tooan Azure Cloud Service, a Visual Studio eszközből](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Hogyan: letöltése és importálása beállítások és az előfizetési adatok közzététele](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
