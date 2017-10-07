---
title: "a felhő mappa tooAzure App Service aaaSync tartalmat"
description: "Ismerje meg, hogyan toodeploy az alkalmazás tooAzure App Service segítségével tartalom szinkronizálása felhő mappából."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: e1c6d53a427c36126d9cdb33cc21b4126b9d9c2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a>Szinkronizálási tartalmat egy felhőalapú mappa tooAzure App Service
Az oktatóanyag bemutatja, hogyan toodeploy túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) által népszerű tárolási felhőszolgáltatások, például a Dropbox és a onedrive vállalati verzió származó tartalom szinkronizálása. 

## <a name="overview"></a>Tartalom sync telepítésének áttekintése
a tárolt tartalom szinkronizálása telepítési hello hello működteti [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki) App Service szolgáltatással integrált. A hello [Azure Portal](https://portal.azure.com), akkor meghatározhat egy mappát a felhőbeli tárolóhelyen, az alkalmazás és a mappában található tartalom, és szinkronizálási tooApp szolgáltatás a hello kattintson gomb. Tartalom szinkronizálási hello Kudu folyamata buildelés és üzembe helyezés használja. 

## <a name="contentsync"></a>Hogyan tooenable tartalom szinkronizálása a központi telepítés
tartalom szinkronizálás tooenable hello [Azure Portal](https://portal.azure.com), kövesse az alábbi lépéseket:

1. A hello Azure portálra az alkalmazás paneljén kattintson **beállítások** > **központi telepítés forrásának**. Kattintson a **forrás választása**, majd jelölje be **OneDrive** vagy **Dropbox** telepítési hello forrásaként. 
   
    ![Tartalom szinkronizálása](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Hello API-k, a mögöttes különbségek miatt **onedrive vállalati verzió** jelenleg nem támogatott. 
   > 
   > 
2. Teljes hello engedélyezési munkafolyamat tooenable App Service tooaccess egy adott előre meghatározott kijelölt elérési út a onedrive-on vagy a Dropbox összes, az App Service tartalmának tárolására.  
    Után engedélyezési hello App Service platform használatával biztosítja a hello beállítás toocreate hello tartalom mappája kijelölt tartalom elérési útját, vagy toochoose a kijelölt tartalom elérési út egy meglévő tartalom mappát. kijelölt hello tartalom útvonalakat az App Service-szinkronizáláshoz használt cloud storage-fiókok hello következők tartoznak:  
   
   * **Onedrive vállalati verzió**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Hello után tartalom kezdeti szinkronizálás hello tartalom szinkronizálási igény szerint az Azure-portálon hello kezdeményezhető. Telepítési előzmények érhető el hello **központi telepítések** panelen.
   
    ![Telepítési előzmények](./media/app-service-deploy-content-sync/onedrive_sync.png)

További információ a Dropbox telepítési érhető el a [központi telepítése a Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 

