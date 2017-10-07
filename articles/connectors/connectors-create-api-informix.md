---
title: "aaaAdd hello Informix-összekötőt a Logic Apps a |} Microsoft Docs"
description: "A REST API paraméterekkel Informix-összekötő áttekintése"
services: 
documentationcenter: 
author: gplarsen
manager: anneta
editor: 
tags: connectors
ms.assetid: ca2393f0-3073-4dc2-8438-747f5bc59689
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: 7a163e2ebf00fa3109b93e34845d922c2174a48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-informix-connector"></a>Hello Informix-összekötő az első lépései
Microsoft összekötő Informix a Logic Apps tooresources tárolt IBM Informix-adatbázishoz csatlakozik. hello Informix összekötő egy Microsoft toocommunicate tooremote Informix server ügyfélszámítógépek tartalmazza a TCP/IP-hálózaton keresztül. Ez magában foglalja a felhőalapú adatbázisok, például a Windows Azure virtualizálási futó IBM Informix és a helyszíni hello helyszíni adatátjáró használó adatbázisok. Lásd: hello [lista támogatott](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix-platformok és-verziói (Ez a témakör).

hello összekötő támogatja az adatbázis-művelet a következő hello:

* Lista adatbázistáblák
* Válasszon használatával egy sor olvasása
* Válassza ki a összes sorok olvasása
* INSERT használatával egy sort ad hozzá
* Egy sor frissítés használatával frissíthető az ALTER
* Egy sor törlése használatával eltávolítása

Ez a témakör bemutatja, hogyan toouse hello összekötőt a logic app tooprocess az adatbázis-műveletek.

További információk a Logic Apps toolearn lásd [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Rendelkezésre álló műveletek
Ez az összekötő támogatja a következő logic app műveletek hello:

* Getables
* GetRow
* GetRows hívás
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Listáját táblákat
Sok lépést végre hello Microsoft Azure portálon keresztül áll semmilyen művelethez logikai alkalmazás létrehozása.

Hello logic App alkalmazásban egy művelet toolist táblák adhat hozzá egy Informix-adatbázishoz. Ez a művelet utasítja hello összekötő tooprocess Informix-schema utasításban, például a `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `InformixgetTables`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**.  
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - Get táblák (előzetes verzió)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. A hello **Informix - Get táblák** konfigurációs ablaktáblán válassza előbb **jelölőnégyzet** tooenable **keresztül, a helyszíni adatátjáró**. Figyelje meg, hogy a felhő tooon helyszíni módosítsa hello-beállítások.
   
   * Írja be a következő **Server**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `ibmserver01:9089`.
   * Írja be a következő **adatbázis**. Írja be például `nwind`.
   * Válassza ki a következő **hitelesítési**. Válassza például **alapvető**.
   * Írja be a következő **felhasználónév**. Írja be például `informix`.
   * Írja be a következő **jelszó**. Írja be például `Password1`.
   * Válassza ki a következő **átjáró**. Válassza például **datagateway01**.
7. Válassza ki **létrehozása**, majd válassza ki **mentése**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. A hello **InformixgetTables** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
9. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_tables**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely olyan táblák listáját.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Hello kapcsolatok létrehozása
Ez az összekötő támogatja kapcsolatok toodatabase a helyszíni és hello felhőben használatával hello következő kapcsolat tulajdonságai. 

| Tulajdonság | Leírás |
| --- | --- |
| kiszolgáló |Kötelező. Egy karakterláncértéket, amely a egy TCP/IP-cím vagy egy aliast IPv4 vagy IPv6 formátumú, majd (kettőspont tagolt) egy TCP/IP-portszám fogad el. |
| adatbázis |Kötelező. Egy karakterláncértéket, amely egy DRDA relációs adatbázis nevét (RDBNAM) fogad el. Informix 128 bájtos karakterlánc fogad el (adatbázis az IBM Informix-adatbázis nevének (dbname) nevezik). |
| Hitelesítés |Választható. Fogadja el a cikk listaértéket, alapszintű vagy a Windows (kerberos). |
| felhasználónév |Kötelező. Egy karakterláncértéket fogad el. |
| jelszó |Kötelező. Egy karakterláncértéket fogad el. |
| Átjáró |Kötelező. A lista elem értékét képviselő hello a helyszíni adatok definiált átjáró tooLogic alkalmazások hello tárolócsoport belül fogad el. |

## <a name="create-hello-on-premises-gateway-connection"></a>Hello a helyszíni átjáró kapcsolat létrehozása
Ez az összekötő hozzáférhet helyszíni adatátjáró hello segítségével helyszíni Informix-adatbázishoz. Átjáró témakörök további információt. 

1. A hello **átjárók** konfigurációs ablaktáblán válassza előbb **jelölőnégyzet** tooenable **átjárón keresztül Connect**. Tekintse meg a beállítások módosítása a felhő tooon helyszíni hello.
2. Írja be a következő **Server**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `ibmserver01:9089`.
3. Írja be a következő **adatbázis**. Írja be például `nwind`.
4. Válassza ki a következő **hitelesítési**. Válassza például **alapvető**.
5. Írja be a következő **felhasználónév**. Írja be például `informix`.
6. Írja be a következő **jelszó**. Írja be például `Password1`.
7. Válassza ki a következő **átjáró**. Válassza például **datagateway01**.
8. Válassza ki **létrehozása** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Hello felhő kapcsolat létrehozása
Ez az összekötő érhetik el a felhő Informix-adatbázishoz. 

1. A hello **átjárók** konfigurációs ablaktáblában hagyja hello **jelölőnégyzet** (rákattintás előtt) le van tiltva **átjárón keresztül Connect**. 
2. Írja be a következő **kapcsolatnév**. Írja be például `hisdemo2`.
3. Írja be a következő **Informix kiszolgálónév**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `hisdemo2.cloudapp.net:9089`.
4. Írja be a következő **Informix adatbázisnév**. Írja be például `nwind`.
5. Írja be a következő **felhasználónév**. Írja be például `informix`.
6. Írja be a következő **jelszó**. Írja be például `Password1`.
7. Válassza ki **létrehozása** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Válassza ki a összes sorok beolvasása
Létrehozhat egy logic app művelet toofetch minden sor hello Informix tábla. Ez a művelet, mint utasítja hello összekötő tooprocess az Informix SELECT utasítás `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve** (pl. "**InformixgetRows**"), **előfizetés**, **erőforráscsoport**, **hely**, és **App Service-csomag**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - Get sorok (előzetes verzió)** .
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**.
7. A hello **kapcsolatok** konfigurációs ablaktáblán válassza előbb **hozzon létre új**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. A hello **átjárók** konfigurációs ablaktáblában hagyja hello **jelölőnégyzet** (rákattintás előtt) le van tiltva **átjárón keresztül Connect**.
   
   * Írja be a következő **kapcsolatnév**. Írja be például `HISDEMO2`.
   * Írja be a következő **Informix kiszolgálónév**, cím- vagy aliasnév kettőspont portszám hello formájában. Írja be például `HISDEMO2.cloudapp.net:9089`.
   * Írja be a következő **Informix adatbázisnév**. Írja be például `NWIND`.
   * Írja be a következő **felhasználónév**. Írja be például `informix`.
   * Írja be a következő **jelszó**. Írja be például `Password1`.
9. Válassza ki **létrehozása** toocontinue.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
11. Bejelölheti **speciális beállítások megjelenítése** toospecify lekérdezési lehetőségek.
12. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. A hello **InformixgetRows** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
14. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely sorok listája.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT használatával egy sort ad hozzá
Egy Informix tábla hozhat létre egy logic app műveleti tooadd egy sor. Ez a művelet, mint utasítja hello összekötő tooprocess egy Informix INSERT utasítás `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `InformixinsertRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - sor beszúrása (előzetes verzió)**.
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációs ablaktáblában válassza tooselect egy kapcsolat. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**, típus `Area 99999`, és írja be `102` a **REGIONID**. 
10. Kattintson a **Mentés** gombra.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. A hello **InformixinsertRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely hello új sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Válasszon használatával egy sort lehívni
Egy Informix tábla hozhat létre egy logic app műveleti toofetch egy sor. Ez a művelet utasítja hello összekötő tooprocess egy Informix ahol válasszon utasítás, például a `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `InformixgetRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - Get sorok (előzetes verzió)** .
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblában válassza tooselect egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**. 
10. Bejelölheti **speciális beállítások megjelenítése** toospecify lekérdezési lehetőségek.
11. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. A hello **InformixgetRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
13. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>FRISSÍTÉS használatával frissíthető egy sor módosítása
Egy Informix tábla hozhat létre egy logic app műveleti toochange egy sor. Ez a művelet utasítja hello összekötő tooprocess Informix utasítást, például a `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `InformixupdateRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - frissítés sor (előzetes verzió)**.
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblában válassza tooselect egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**, típus `Updated 99999`, és írja be `102` a **REGIONID**. 
10. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. A hello **InformixupdateRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, illetve nézet hello kimenete, amely hello új sort kell tartalmaznia.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Egy sor törlése használatával eltávolítása
Egy Informix tábla hozhat létre egy logic app műveleti tooremove egy sor. Ez a művelet, mint utasítja hello összekötő tooprocess egy Informix DELETE utasítás `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása
1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.
2. Adja meg a hello **neve**, például a `InformixdeleteRow`, **előfizetés**, **erőforráscsoport**, **hely**, és **alkalmazás Terv szolgáltatás**. Válassza ki **PIN-kód toodashboard**, majd válassza ki **létrehozása**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá egy eseményindító és művelet
1. A hello **Logic Apps Designer**, jelölje be **üres LogicApp** a hello **sablonok** listája.
2. A hello **eseményindítók** listáról válassza ki **ismétlődési**. 
3. A hello **ismétlődési** eseményindító, jelölje be **szerkesztése**, jelölje be **gyakoriság** legördülő tooselect **nap**, majd válassza ki  **Időköz** tootype **7**. 
4. Jelölje be hello **+ új lépés** mezőbe, majd válassza ki **művelet hozzáadása**.
5. A hello **műveletek** kilistázásához írja be **informix** a hello **keresse meg a további műveletek** szerkesztési mezőhöz, és válassza **Informix - sor törlése (előzetes verzió)**.
6. A hello **első sort (előzetes verzió)** művelet, jelölje be **kapcsolat módosítása**. 
7. A hello **kapcsolatok** konfigurációk ablaktáblán válassza ki egy létező kapcsolatot. Válassza például **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. A hello **táblanév** listában, jelölje be hello **lefelé mutató nyíl**, majd válassza ki **terület**.
9. Adja meg (lásd a piros csillaggal jelölt) minden szükséges oszlopot. Írja be például `99999` a **AREAID**. 
10. Kattintson a **Mentés** gombra. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. A hello **InformixdeleteRow** panelen belül hello **minden futtatásakor** listában az **összegzés**, válasszon hello első felsorolt elem (legutóbbi futtatás).
12. A hello **logikai alkalmazás futtatása** panelen válassza **futtatása részletek**. Hello belül **művelet** listáról válassza ki **Get_rows**. Lásd: hello értéke **állapot**, kell lennie, amely **sikeres**. Jelölje be hello **bemenetek hivatkozás** tooview hello bemeneti adatok. Jelölje be hello **kimenetek hivatkozás**, és a nézet hello kimenete; kell tartalmaznia, amely hello törölt sor.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Támogatott platformok Informix és verziói
Ez az összekötő támogatja konfigurálásakor a következő IBM Informix verziók hello toosupport elosztott relációs adatbázis architektúra (DRDA) ügyfélkapcsolatokat.

* IBM Informix 12.1
* IBM Informix 11.7

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/informix/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).

