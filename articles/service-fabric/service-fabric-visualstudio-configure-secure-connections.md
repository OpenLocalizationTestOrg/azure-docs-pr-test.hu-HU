---
title: "biztonságos Azure Service Fabric fürt kapcsolatok aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Visual Studio tooconfigure biztonságos kapcsolatok hello Azure Service Fabric-fürt által támogatott."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: ed3e5043264cf026f74e24ca09b40ccc70086cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a>Biztonságos kapcsolat tooa Service Fabric-fürt a Visual Studio konfigurálása
Ismerje meg, hogyan toouse Visual Studio toosecurely elérni az Azure Service Fabric-fürt beállított hozzáférés-vezérlési házirendeket.

## <a name="cluster-connection-types"></a>Fürt-kapcsolat típusai
Kétféle típusú kapcsolatok hello Azure Service Fabric-fürt által támogatott: **nem biztonságos** kapcsolatok és **tanúsítványalapú x509** biztonságos kapcsolatokat. (A Service Fabric-fürtök helyben tárolt, **Windows** és **dSTS** hitelesítések is támogatottak.) Hello fürt létrehozásakor rendelkezik tooconfigure hello fürt kapcsolattípus. A már létrehozott, hello kapcsolat típusa nem módosítható.

hello Visual Studio Service Fabric-eszközök támogatása közzétételi csatlakozó tooa fürt összes hitelesítéstípussal. Lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md) útmutatást tooset biztonságos Service Fabric-fürt.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Fürt kapcsolatok konfigurálása a profilok közzététele
Ha közzéteszi a Service Fabric-projektet, a Visual Studio, használja a hello **Fabric-alkalmazás közzététele** párbeszédpanel bezárásához toochoose egy Azure Service Fabric-fürt. A **csatlakozási végpont**, jelölje be az előfizetéshez tartozó meglévő fürt.

![hello ** közzététele Service Fabric Application ** párbeszédpanel használt tooconfigure a Service Fabric kapcsolat.][publishdialog]

Hello **Fabric-alkalmazás közzététele** párbeszédpanel automatikusan ellenőrzi az hello fürtkapcsolat. Ha a rendszer kéri, jelentkezzen be Azure-fiók tooyour. Ha az ellenőrzés eredménye akkor megfelelő, az azt jelenti, hogy a rendszer hello megfelelő tanúsítványok telepítve tooconnect toohello fürt biztonságosan, vagy a fürt nem biztonságos. Az érvényesítési hibák okozhatja hálózati probléma, vagy nem rendelkezik a rendszer megfelelően konfigurált tooconnect tooa biztonságos fürt.

![hello ** közzététele Service Fabric alkalmazás ** párbeszédpanel ellenőrzi, hogy egy meglévő, megfelelően konfigurálva a Service Fabric-fürt kapcsolat.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a>tooconnect tooa biztonságos fürt
1. Ellenőrizze, hogy a cél fürt bizalmi kapcsolatok hello hello ügyféltanúsítványok hozzáférhet. hello tanúsítvány általában megosztott, személyes információcsere (.pfx) fájlként. Lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md) hogyan férhetnek hozzá a tooconfigure hello server toogrant tooa ügyfél számára.
2. Hello megbízható tanúsítvány telepítéséhez. toodo, kattintson duplán a hello .pfx fájlt, vagy hello PowerShell parancsfájl Import-PfxCertificate tooimport hello tanúsítványokat használnak. Hello tanúsítvány telepítése túl**Cert: \LocalMachine\My**. OK tooaccept hello tanúsítvány importálása során az összes alapértelmezett beállítást.
3. Válassza ki a hello **közzététel...**  hello helyi menüjének hello projekt tooopen hello parancs **Azure-alkalmazás közzététele** párbeszédpanel megnyitásához, majd jelölje be hello cél fürtnek. hello eszköz automatikusan feloldja a hello kapcsolat, és menti hello biztonságos kapcsolatot hello paramétereiben közzététele profil.
4. Választható lehetőség: Hello szerkesztheti profil toospecify biztonságos fürtkapcsolat közzététele.
   
   Manuálisan szerkesztése hello közzététele profil XML fájl toospecify hello tanúsítvány adatait, mert meg arról, hogy toonote hello tanúsítványtároló-nevet, tárolja a hely és a tanúsítvány ujjlenyomata. Ezeket az értékeket hello tanúsítványok tárába nevet, és tárolóhelyére tooprovide lesz szüksége. Lásd: [hogyan: lekérése hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) további információt.
   
   Használhatja a hello *ClusterConnectionParameters* paraméterek toospecify hello PowerShell paraméterek toouse toohello Service Fabric-fürt kapcsolódáskor. Érvényes paraméterei bármilyen, amely hello Connect-ServiceFabricCluster parancsmag elfogadja. Lásd: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) felhasználható paraméterek listáját.
   
   Ha tooa távoli fürt még közzétételt, toospecify hello megfelelő paraméterekkel kell, hogy a fürt. hello következő az összekötő tooa nem biztonságos fürt:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Íme egy példa a kapcsolódó tooan x509, tanúsítvány-alapú biztonságos fürt:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Bármely más szükséges beállításokat, például a frissítési paraméterek és az alkalmazás paraméter fájl helye, szerkesztheti, és tegye közzé az alkalmazást a hello **Fabric-alkalmazás közzététele** párbeszédpanel a Visual Studióban.

## <a name="next-steps"></a>Következő lépések
Service Fabric-fürtök elérésével kapcsolatos további információkért lásd: [a fürt megjelenítése a Service Fabric Explorer használatával](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
