---
title: "aaaAdd hello DB2-összekötőt a Logic Apps a |} Microsoft Docs"
description: "A REST API paraméterekkel DB2-összekötő áttekintése"
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: d836c61231d3c9cfdb30ff9c40a48b1718f3ffb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-db2-connector"></a>Hello DB2-összekötő az első lépései
Microsoft DB2-összekötő kapcsolódik a Logic Apps tooresources IBM DB2-adatbázisban tárolják. Ez az összekötő tartalmazza a Microsoft ügyfél toocommunicate DB2-kiszolgáló távoli számítógépek TCP/IP-hálózaton keresztül. Ez magában foglalja a felhőalapú adatbázisok, például az IBM Bluemix dashDB vagy a Windows Azure virtualizálási futó IBM DB2 és a helyszíni hello helyszíni adatátjáró használó adatbázisok. Lásd: hello [lista támogatott](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-platformok és-verziói (Ez a témakör).

hello DB2-összekötő a következő adatbázis-műveletek hello támogatja:

* Lista adatbázistáblák
* Válasszon használatával egy sor olvasása
* Válassza ki a összes sorok olvasása
* INSERT használatával egy sort ad hozzá
* Egy sor frissítés használatával frissíthető az ALTER
* Egy sor törlése használatával eltávolítása

Ez a témakör bemutatja, hogyan toouse hello összekötőt a logic app tooprocess az adatbázis-műveletek.

További információk a Logic Apps toolearn lásd [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Rendelkezésre álló műveletek
hello DB2-összekötő a következő logic app műveletek hello támogatja:

* GetTables
* GetRow
* GetRows hívás
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Listáját táblákat
Sok lépést végre hello Microsoft Azure portálon keresztül áll semmilyen művelethez logikai alkalmazás létrehozása.

Hello logic App alkalmazásban egy művelet toolist táblák adhat hozzá egy DB2-adatbázishoz. hello művelet arra utasítja a hello összekötő tooprocess DB2 schema utasításban, mint például `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `Db2getTables`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, hello ,majdállítsabe**Időköz** tootype **7**.  
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be `db2` a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - Get táblák (előzetes verzió)**.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. A hello **DB2 - Get táblák** konfigurációs ablaktáblán válassza előbb **jelölőnégyzet** tooenable **keresztül, a helyszíni adatátjáró**. Figyelje meg, hogy a felhő tooon helyszíni módosítsa hello-beállítások.
   
   * Írja be a következő **Server**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `ibmserver01:50000`.
   * Írja be a következő **adatbázis**. Írja be például `nwind`.
   * Válassza ki a következő **hitelesítési**. Válassza például **alapvető**.
   * Írja be a következő **felhasználónév**. Írja be például `db2admin`.
   * Írja be a következő **jelszó**. Írja be például `Password1`.
   * Válassza ki a következő **átjáró**. Válassza például **datagateway01**.
7. Válassza ki **létrehozása**, majd válassza ki **mentése**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. A hello **Db2getTables** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
9. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_tables**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely olyan táblák listáját.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Hello kapcsolatok létrehozása
Ez az összekötő támogatja kapcsolatok üzemeltetett toodatabases a helyszíni és hello felhőben használatával hello következő kapcsolat tulajdonságai. 

| Tulajdonság | Leírás |
| --- | --- |
| kiszolgáló |Kötelező. Egy karakterláncértéket, amely egy TCP/IP-cím vagy az alias, IPv4 vagy IPv6 formátumú, majd (pontosvesszővel elválasztott) egy TCP/IP-portszám fogad el. |
| adatbázis |Kötelező. Fogadja el a DRDA relációs adatbázis nevét (RDBNAM) jelölő karakterlánc-érték. Z/os DB2 fogad el egy 16 bájtos karakterlánc (adatbázis egy IBM DB2 z/OS helyének nevezzük). DB2 i5/os fogad el egy 18-többbájtos karakterlánc (adatbázis egy IBM DB2 a néven relációs i adatbázis). A LUW DB2 egy 8 bájtos karakterlánc fogad el. |
| Hitelesítés |Választható. Fogadja el a cikk listaértéket, alapszintű vagy a Windows (kerberos). |
| felhasználónév |Kötelező. Egy karakterláncértéket fogad el. Z/os DB2 egy 8 bájtos karakterlánc fogad el. DB2 az i fogad el egy 10 – többbájtos karakterlánc. A Linux vagy UNIX DB2 egy 8 bájtos karakterlánc fogad el. DB2 Windows 30 bájtos karakterlánc fogad el. |
| jelszó |Kötelező. Egy karakterláncértéket fogad el. |
| Átjáró |Kötelező. A lista elem értékét képviselő hello a helyszíni adatok definiált átjáró tooLogic alkalmazások hello tárolócsoport belül fogad el. |

## <a name="create-hello-on-premises-gateway-connection"></a>Hello a helyszíni átjáró kapcsolat létrehozása
Ez az összekötő egy helyszíni DB2-adatbázishoz hello helyszíni átjáró hozzáférhet. Átjáró témakörök további információt. 

1. A hello **átjárók** konfigurációs ablaktáblán válassza előbb **jelölőnégyzet** tooenable **átjárón keresztül Connect**. Figyelje meg, hogy a felhő tooon helyszíni módosítsa hello-beállítások.
2. Írja be a következő **Server**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `ibmserver01:50000`.
3. Írja be a következő **adatbázis**. Írja be például `nwind`.
4. Válassza ki a következő **hitelesítési**. Válassza például **alapvető**.
5. Írja be a következő **felhasználónév**. Írja be például `db2admin`.
6. Írja be a következő **jelszó**. Írja be például `Password1`.
7. Válassza ki a következő **átjáró**. Válassza például **datagateway01**.
8. Válassza ki **létrehozása** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Hello felhő kapcsolat létrehozása
Az összekötő hozzáférhessen egy felhő DB2-adatbázishoz. 

1. A hello **átjárók** konfigurációs ablaktáblában hagyja hello **jelölőnégyzet** (rákattintás előtt) le van tiltva **átjárón keresztül Connect**. 
2. Írja be a következő **kapcsolatnév**. Írja be például `hisdemo2`.
3. Írja be a következő **DB2 kiszolgálónév**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `hisdemo2.cloudapp.net:50000`.
4. Írja be a következő **DB2 adatbázisnév**. Írja be például `nwind`.
5. Írja be a következő **felhasználónév**. Írja be például `db2admin`.
6. Írja be a következő **jelszó**. Írja be például `Password1`.
7. Válassza ki **létrehozása** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Válassza ki a összes sorok beolvasása
Megadhatja a logic app művelet toofetch minden sor egy DB2-tábla. Ez arra utasítja a hello összekötő tooprocess egy DB2 SELECT utasítás például `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `Db2getRows`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be `db2` a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - Get sorok (előzetes verzió)**.
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**.
7. A hello **kapcsolatok** konfigurációs ablaktáblán válassza előbb **hozzon létre új**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. A hello **átjárók** konfigurációs ablaktáblában hagyja hello **jelölőnégyzet** (rákattintás előtt) le van tiltva **átjárón keresztül Connect**.
   
   * Írja be a következő **kapcsolatnév**. Írja be például `HISDEMO2`.
   * Írja be a következő **DB2 kiszolgálónév**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `HISDEMO2.cloudapp.net:50000`.
   * Írja be a következő **DB2 adatbázisnév**. Írja be például `NWIND`.
   * Írja be a következő **felhasználónév**. Írja be például `db2admin`.
   * Írja be a következő **jelszó**. Írja be például `Password1`.
9. Válassza ki **létrehozása** toocontinue.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
11. Bejelölheti **speciális beállítások megjelenítése** toospecify lekérdezési lehetőségek.
12. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. A hello **Db2getRows** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
14. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely sorok listája.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT használatával egy sort ad hozzá
A logic app műveleti tooadd egy sor definiálhat egy DB2-tábla. Ez a művelet utasítja hello összekötő tooprocess a DB2 INSERT utasításban, mint `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `Db2insertRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **db2** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - (előzetes verzió) sor beszúrása**.
6. A hello **DB2 - (előzetes verzió) sor beszúrása** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációs ablaktáblán válasszon ki egy kapcsolattípust. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**, típus `Area 99999`, és írja be `102` a **REGIONID**. 
10. Kattintson a **Mentés** gombra.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. A hello **Db2insertRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely hello új sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Válasszon használatával egy sort lehívni
A logic app műveleti toofetch egy sor definiálhat egy DB2-tábla. Ez a művelet utasítja hello összekötő tooprocess egy DB2 ahol válasszon utasítás, például a `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve** (pl. "**Db2getRow**"), **előfizetés**, **erőforráscsoport**, **hely**, és **App Service-csomag**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája. 
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **db2** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - Get sorok (előzetes verzió)**.
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblán válassza ki egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**. 
10. Bejelölheti **speciális beállítások megjelenítése** toospecify lekérdezési lehetőségek.
11. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. A hello **Db2getRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
13. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>FRISSÍTÉS használatával frissíthető egy sor módosítása
A logic app műveleti toochange egy sor definiálhat egy DB2-tábla. Ez a művelet utasítja hello összekötő tooprocess DB2 utasítást, például a `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `Db2updateRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **db2** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - frissítés sor (előzetes verzió)**.
6. A hello **DB2 - frissítés sor (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblában válassza tooselect egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**, típus `Updated 99999`, és írja be `102` a **REGIONID**. 
10. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. A hello **Db2updateRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely hello új sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Egy sor törlése használatával eltávolítása
A logic app műveleti tooremove egy sor definiálhat egy DB2-tábla. Ez a művelet, mint utasítja hello összekötő tooprocess egy DB2 DELETE utasítás `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `Db2deleteRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája. 
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** listáról válassza ki **db2** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **DB2 - sor törlése (előzetes verzió)**.
6. A hello **DB2 - sor törlése (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblán válassza ki egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**. 
10. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. A hello **Db2deleteRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely hello törölt sor.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="supported-db2-platforms-and-versions"></a>Támogatott platformok DB2 és verziók
Ez az összekötő támogatja hello IBM DB2-platformok és verziók, valamint az IBM DB2 kompatibilis termékek (pl. IBM Bluemix dashDB), amely elosztott relációs adatbázis architektúra (DRDA) SQL Access Manager (SQLAM) 10 vagy 11 verziója támogatja a következő:

* IBM DB2 z/os 11.1
* IBM DB2 z/OS 10.1
* Az IBM DB2 i 7.3.
* Az IBM DB2 i 7.2
* Az IBM DB2 i 7.1
* IBM DB2 LUW 11
* IBM DB2 LUW 10.5 számára

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/db2/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).

