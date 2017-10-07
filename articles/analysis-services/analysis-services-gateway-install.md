---
title: "a helyszíni adatátjáró aaaInstall |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és konfiguráljon egy a helyszíni adatok átjárót."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Telepítse és konfigurálja a helyszíni adatátjáró
Egy a helyszíni adatok átjáróra szükség, ha az egy vagy több Azure Analysis Services-kiszolgáló azonos hello régió tooon helyszíni adatforrások csatlakoztatásához. toolearn hello átjáró kapcsolatos további információkért lásd: [helyszíni adatátjáró](analysis-services-gateway.md).

## <a name="prerequisites"></a>Előfeltételek
**Minimumkövetelmények:**

* .NET 4.5 keretrendszer
* 64 bites Windows 7 vagy Windows Server 2008 R2 (vagy újabb)

**Ajánlott:**

* 8 mag Processzor
* 8 GB memória
* 64 bites Windows 2012 R2 (vagy újabb)

**Fontos tudnivalók találhatók:**

* A telepítés során, ha az átjáró regisztrálása az Azure-ral hello alapértelmezett terület előfizetési van kiválasztva. Lehetősége van egy másik régióban. Ha egynél több régióban kiszolgálóval rendelkezik, telepítenie kell egy átjáró mindegyik régióhoz. 
* hello átjáró nem telepíthető tartományvezérlőre.
* Csak egy átjáró telepíthető egy számítógépen.
* Hello átjáró telepíthető olyan számítógépre, amely továbbra is megtalálható, és nem halad toosleep.
* Ne telepítse a számítógép vezeték nélkül csatlakozó tooyour hálózat hello átjárót. Teljesítmény is lehet csökken.


## <a name="download"></a>Letöltése
 [Töltse le a hello átjáró](https://aka.ms/azureasgateway)

## <a name="install"></a>Telepítése

1. Futtassa a telepítőt.

2. Jelöljön ki egy helyet, hello fogadnia, és kattintson **telepítése**.

   ![Telepítse a helyét és a licencszerződés feltételeit](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Válassza ki **(ajánlott) a helyszíni adatátjáró**. Az Azure Analysis Services nem támogatja a személyes módban.

   ![Válassza ki az átjáró típusa](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. Adjon meg egy fiókot toosign tooAzure. hello fióknak kell lennie a bérlő Azure Active Directoryban. Ez a fiók hello átjáró rendszergazdája szolgál. 

   ![Adjon meg egy fiókot toosign tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Ha egy tartományi fiókkal jelentkezik, akkor lesz leképezve tooyour szervezeti fiókkal az Azure ad-ben. A szervezeti fiókjával hello hello átjáró rendszergazdája lesz.

## <a name="register"></a>Regisztráció
A sorrend toocreate egy átjáró-erőforráshoz az Azure-ban regisztrálnia kell a hello helyi példány hello átjáró Felhőszolgáltatáshoz telepítette. 

1.  Válassza ki **ezen a számítógépen egy új átjáró regisztrálása**.

    ![Regisztráljon](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Írja be az átjáró nevét és a helyreállítási kulcsot. Hello átjáró alapértelmezés szerint az előfizetés alapértelmezett régió használja. Ha egy másik régióban tooselect van szüksége, válassza ki a **régió módosítása**.

   ![Regisztráljon](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Hozzon létre egy Azure átjáró erőforrást
Miután telepíteni és regisztrálni az átjárót, az Azure-előfizetéshez toocreate egy átjáró-erőforráshoz kell. Jelentkezzen be a hello tooAzure azonos hello átjáró regisztrálásakor használt fiókkal.

1. Az Azure portálon kattintson **hozzon létre egy új** > **vállalati integrációs** > **helyszíni adatátjáró** > **létrehozása**.

   ![Hozzon létre egy átjáró erőforrást](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. A **kapcsolat átjáró létrehozása**, adja meg ezeket a beállításokat:

    * **Név**: Adjon meg egy nevet az átjáró-erőforráshoz. 

    * **Előfizetés**: Válasszon hello Azure-előfizetés tooassociate az átjáró-erőforráshoz. 
    Ehhez az előfizetéshez kell hello a kiszolgálók ugyanazt az előfizetést.
   
      hello alapértelmezett előfizetés hello Azure-fiókot, amelyet a toosign használt alapul.

    * **Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy már meglévőt.

    * **Hely**: Select hello régió regisztrálta az átjáró található.

    * **Telepítési neve**: Ha nincs kiválasztva, akkor az átjáró telepítésének, jelölje be hello átjáró regisztrálva. 

    Amikor elkészült, kattintson a **létrehozása**.

## <a name="connect-servers"></a>Csatlakoztassa az kiszolgálók toohello átjáró erőforrást

1. Kattintson az Azure Analysis Services-kiszolgáló áttekintése **helyszíni adatátjáró**.

   ![Csatlakozás kiszolgálóhoz toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. A **válasszon egy helyszíni Data Gateway tooconnect**, jelölje be az átjáró-erőforráshoz, és kattintson a **csatlakozás a kiválasztott átjáróeszközön**.

   ![Csatlakozás kiszolgálóhoz toogateway erőforrás](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Az átjáró hello lista nem jelenik meg, ha a kiszolgáló nincs valószínűleg az hello és hello átjáró regisztrálásakor megadott hello régió ugyanabban a régióban. 

Ennyi az egész. Ha tooopen portokat kell vagy hibaelhárításra tegye, lehet, hogy toocheck kimenő [helyszíni adatátjáró](analysis-services-gateway.md).

## <a name="next-steps"></a>Következő lépések
* [Analysis Services kezelése](analysis-services-manage.md)   
* [Adatok beolvasása az Azure Analysis Services](analysis-services-connect.md)
