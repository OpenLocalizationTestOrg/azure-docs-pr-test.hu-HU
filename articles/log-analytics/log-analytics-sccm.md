---
title: a Configuration Manager tooLog Analytics aaaConnect |} Microsoft Docs
description: "Ez a cikk bemutatja a hello lépéseket tooconnect Configuration Manager tooLog, elemzés és a start-adatok elemzése."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: dc50ebc46020a806d99d1a3e3d0e91fd09ad2c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-configuration-manager-toolog-analytics"></a>Csatlakozás a Configuration Manager tooLog elemzés
System Center Configuration Manager tooLog Analytics az OMS toosync eszköz gyűjtemény adatait is elérheti. Így az adatok a Configuration Manager hierarchiából OMS érhető el.

## <a name="prerequisites"></a>Előfeltételek

A Naplóelemzési támogatja a System Center Configuration Manager aktuális ágának, 1606 vagy újabb verziója.  

## <a name="configuration-overview"></a>Konfigurálása – áttekintés
a lépéseket követve hello hello folyamat tooconnect Configuration Manager tooLog Analytics foglalja össze.  

1. Hello Azure felügyeleti portálon, a Configuration Manager regisztrálható egy webalkalmazás és/vagy webes API-alkalmazást, és győződjön meg arról, hogy rendelkezik-e hello azonosító és az ügyfél titkos ügyfélkulcsot hello regisztráció az Azure Active Directoryból származó. Lásd: [portál toocreate Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez használjon](../azure-resource-manager/resource-group-create-service-principal-portal.md) kapcsolatos részletes információkat ebben a lépésben megvalósításához.
2. Az Azure felügyeleti portálon, hello [adjon meg a Configuration Manager (hello regisztrált webalkalmazás) engedély tooaccess OMS](#provide-configuration-manager-with-permissions-to-oms).
3. A Configuration Managerben [hello Hozzáadás OMS kapcsolat varázsló használatával vegye fel](#add-an-oms-connection-to-configuration-manager).
4. A Configuration Managerben [hello kapcsolat tulajdonságainak frissítéséhez](#update-oms-connection-properties) Ha hello jelszó vagy ügyfél titkos kulcs soha lejár vagy elvész.
5. Hello OMS-portálon adataival [töltse le és telepítse a Microsoft Monitoring Agent hello](#download-and-install-the-agent) hello futtató hello Configuration Manager szolgáltatáskapcsolódási pont helyrendszerszerepkör. hello ügynök Configuration Manager adatok tooOMS küld.
6. A Naplóelemzési [gyűjteményeket importálhat a Configuration Manager](#import-collections) , számítógépcsoportokat.
7. A Naplóelemzési, megtekintheti az adatokat a Configuration Manager alkalmazásból, [számítógépcsoportok](log-analytics-computer-groups.md).

További, a Configuration Manager tooOMS összekapcsolásával kapcsolatos [szinkronizálni az adatokat a Configuration Manager toohello a Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-toooms"></a>Adja meg a Configuration Manager az engedélyek tooOMS
hello alábbi eljárás ismerteti hello Azure felügyeleti portálon az engedélyek tooaccess OMS Szolgáltatáshoz. Pontosabban, biztosítania kell a hello *közreműködői szerepkör* hello erőforráscsoportban a rendelés tooallow toousers hello Azure felügyeleti portálon tooconnect Configuration Manager tooOMS.

> [!NOTE]
> A Configuration Manager OMS engedélyeket kell megadnia. Ellenkező esetben egy hibaüzenetet fog kapni, a Configuration Manager hello konfigurálása varázsló használatakor.
>
>

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com/) kattintson **Tallózás** > **Naplóelemzés (OMS)** tooopen hello Naplóelemzés (OMS) panelen.  
2. A hello **Naplóelemzés (OMS)** panelen kattintson a **Hozzáadás** tooopen hello **OMS-munkaterület** panelen.  
   ![OMS panel](./media/log-analytics-sccm/sccm-azure01.png)
3. A hello **OMS-munkaterület** panelen adja meg a következő információ hello, és kattintson a **OK**.

   * **OMS-munkaterület**
   * **Előfizetés**
   * **Erőforráscsoport**
   * **Hely**
   * **Tarifacsomag**  
     ![OMS panel](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > hello fenti példa létrehoz egy új erőforráscsoportot. hello erőforráscsoport csak használt tooprovide Configuration Manager az engedélyek toohello OMS-munkaterület ebben a példában egy.
     >
     >
4. Kattintson a **Tallózás** > **erőforráscsoportok** tooopen hello **erőforráscsoportok** panelen.
5. A hello **erőforráscsoportok** panelen kattintson a fenti tooopen hello létrehozott erőforráscsoport hello &lt;erőforráscsoport-név&gt; beállítások panelen.  
   ![Erőforráscsoport beállítások panel](./media/log-analytics-sccm/sccm-azure03.png)
6. A hello &lt;erőforráscsoport-név&gt; beállítási paneljén kattintson Access control (IAM) tooopen hello &lt;erőforráscsoport-név&gt; felhasználók panel.  
   ![Erőforráscsoport felhasználók panel](./media/log-analytics-sccm/sccm-azure04.png)  
7. A hello &lt;erőforráscsoport-név&gt; felhasználók panel, kattintson a **Hozzáadás** tooopen hello **hozzáférés hozzáadása** panelen.
8. A hello **hozzáférés hozzáadása** panelen kattintson **szerepkör kiválasztása**, majd válassza ki a hello **közreműködői** szerepkör.  
   ![szerepkör kiválasztása](./media/log-analytics-sccm/sccm-azure05.png)  
9. Kattintson a **felhasználók hozzáadása az**válassza ki a Configuration Manager-felhasználó hello **kiválasztása**, és kattintson a **OK**.  
   ![felhasználók hozzáadása](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-tooconfiguration-manager"></a>Az OMS-kapcsolat tooConfiguration kezelő hozzáadása
Rendelés tooadd az OMS-kapcsolat, a Configuration Manager-környezettel rendelkezik egy [szolgáltatáskapcsolódási pont](https://technet.microsoft.com/library/mt627781.aspx) online módot.

1. A hello **felügyeleti** munkaterület a Configuration Manager, válassza ki **OMS összekötő**. Ekkor megnyílik a hello **OMS kapcsolat varázsló**. Válassza ki **következő**.
2. A hello **általános** képernyőjén ellenőrizze, hogy elvégezte-e a következő műveletek hello és, hogy az egyes elemekről részletek rendelkezik, majd válassza ki, **következő**.

   1. A hello Azure felügyeleti portálon, a webalkalmazás és/vagy webes API-alkalmazás már regisztrálta a Configuration Manager, és hogy rendelkezik hello [hello regisztrációs az ügyfél-azonosító](../active-directory/active-directory-integrating-applications.md).
   2. Hello Azure felügyeleti portálon, a létrehozott egy alkalmazás titkos kulcs hello regisztrált alkalmazás Azure Active Directoryban.  
   3. Hello Azure felügyeleti portálon, a megadott hello regisztrált webalkalmazás az engedély tooaccess OMS Szolgáltatáshoz.  
      ![Csatlakozási tooOMS varázsló Általános lapja](./media/log-analytics-sccm/sccm-console-general01.png)
3. A hello **Azure Active Directory** képernyőn, a kapcsolat beállításait tooOMS konfigurálása azáltal, hogy a **bérlői** , **ügyfél-azonosító** , és **ügyfél titkos kulcs**  , majd jelölje be **következő**.  
   ![Csatlakozási tooOMS Azure Active Directory varázsló lapja](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Ha Ön valósítható meg minden hello más eljárások sikeresen, majd hello hello információk **OMS-kapcsolat konfigurációs** képernyő automatikusan meg fog jelenni ezen a lapon. Hello kapcsolatbeállítások adatait meg kell jelennie a a **Azure-előfizetés** , **Azure erőforráscsoport** , és **Operations Management Suite-munkaterület**.  
   ![Csatlakozási tooOMS OMS-kapcsolat varázsló lapja](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. hello varázsló toohello OMS szolgáltatás fiókjához megadott hello adatainak felhasználásával csatlakozik. Válassza ki a hello eszközgyűjtemények szeretné, hogy az OMS toosync, és kattintson a **Hozzáadás**.  
   ![Gyűjtemények kiválasztása](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Ellenőrizze a kapcsolati beállításokat a hello **összegzés** képernyőt, majd válassza ki **következő**. Hello **folyamatban** képernyő hello kapcsolat állapotát jeleníti meg, majd kell **Complete**.

> [!NOTE]
> A hierarchiában lévő OMS toohello legfelső szintű helyet kell csatlakoztatni. Csatlakozás az OMS tooa önálló elsődleges hely, és vegye fel a központi adminisztrációs hely tooyour környezetekben, ha lesz toodelete rendelkezik, és hozza létre újra hello OMS kapcsolatot hello új hierarchiában.
>
>

Miután a Configuration Manager tooOMS, vegye fel vagy távolítsa el a gyűjtemények és hello OMS kapcsolat hello tulajdonságainak megtekintése.

## <a name="update-oms-connection-properties"></a>OMS kapcsolat tulajdonságainak frissítéséhez
Ha egy jelszót vagy az ügyfél titkos kulcs soha lejár vagy elvész, szüksége lesz a toomanually frissítés hello OMS kapcsolat tulajdonságai.

1. Túl keresse meg a Configuration Managerben**Felhőszolgáltatások** , majd jelölje be **OMS-összekötő** tooopen hello **OMS kapcsolat tulajdonságai** lap.
2. Ezen a lapon kattintson a hello **Azure Active Directory** tooview lapon a **bérlői**, **ügyfél-azonosító**, **ügyfél titkos kulcs lejárati**. **Győződjön meg arról** a **titkos ügyfélkulcsot** , ha már lejárt.

## <a name="download-and-install-hello-agent"></a>Töltse le és telepítse a hello ügynök
1. Az OMS-portálon hello [letöltési hello ügynök telepítőfájl az OMS Szolgáltatáshoz](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. A következő módszerek tooinstall hello valamelyikével, és konfigurálja a hello ügynök hello futtató hello Configuration Manager szolgáltatási kapcsolódási pont helyrendszer-szerepkör:
   * [A telepítőprogram használatával hello ügynök telepítése](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Hello parancssorból hello ügynök telepítése](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Azure Automation DSC használata hello ügynök telepítése](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Gyűjtemények importálása
Az OMS-kapcsolat tooConfiguration Manager hozzáadott és hello ügynök telepítése után hello futtató hello Configuration Manager szolgáltatáskapcsolódási pont helyrendszerszerepkör, hello következő lépés a Configuration Manager az OMS tooimport gyűjtemények számítógép-csoport.

Hello gyűjtemény tagsági információk nélkül importálását engedélyezése után minden 3 óra tookeep hello gyűjteménytagságok aktuális. Kiválaszthatja, hogy bármikor toodisable importálását.

1. Az OMS-portálon hello kattintson **beállítások**.
2. Kattintson a hello **számítógépcsoportok** fülre, majd hello **SCCM** fülre.
3. Válassza ki **importálása a Configuration Manager gyűjteménytagságok** majd **mentése**.  
   ![Számítógépcsoportok - SCCM lap](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>A Configuration Manager alkalmazásból adatok megtekintése
Az OMS-kapcsolat tooConfiguration Manager hozzáadott és hello ügynök hello Configuration Manager szolgáltatási kapcsolódási pont helyrendszerszerepkört futtató hello számítógépre telepített, után hello ügynöktől adatküldést tooOMS. Az OMS-ben, a Configuration Manager-gyűjtemények jelennek meg [számítógépcsoportok](log-analytics-computer-groups.md). Megtekintheti a csoportok hello hello **Configuration Manager** lapon az **számítógépcsoportok** a **beállítások**.

Után hello gyűjtemények importálása tekintheti meg a gyűjtemény tagságát, hogy hány számítógépen észlelt. Megtekintheti a gyűjtemények importálása hello száma is.

![Számítógépcsoportok - SCCM lap](./media/log-analytics-sccm/sccm-computer-groups02.png)

Vagy egy, megnyílik a keresés, csoportok vagy tooeach csoporthoz tartozó összes számítógép hello vagy összes megjelenítése importálva. Használatával [naplófájl-keresési](log-analytics-log-searches.md), elindíthatja a Configuration Manager-adatokat részletes elemzéséhez.

## <a name="next-steps"></a>Következő lépések
* Használjon [naplófájl-keresési](log-analytics-log-searches.md) tooview részletes adatainak megtekintése a Configuration Manager-adatokat.
