---
title: "az OMS IT Service Management-összekötő aaaITSM kapcsolatok |} Microsoft Docs"
description: "Csatlakozás a ITSM termékek vagy szolgáltatások IT Service Management-összekötő OMS toocentrally figyelőben és hello ITSM munkaelemek kezelésére."
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: v-jysur
ms.openlocfilehash: 53ef51bf75fb8ed15ea3ce5072d9365c221f9f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Csatlakozás ITSM termékek vagy szolgáltatások IT Service Management Connector (előzetes verzió)
Ez a cikk nyújt információt tooconnect a ITSM termék vagy szolgáltatás tooIT Service Management-összekötő OMS és központilag a munkaelemek kezelésére. További információ a IT Service Management-összekötő, lásd: [áttekintése](log-analytics-itsmc-overview.md).

a következő termékek vagy szolgáltatások hello támogatottak:

- [A System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [A ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-tooit-service-management-connector-in-oms"></a>Csatlakozás a System Center Service Manager tooIT OMS a Service Management-összekötő

hello alábbi szakaszok nyújtanak részletes tájékoztatást tooconnect a System Center Service Manager termék toohello IT Service Management-összekötő az OMS Szolgáltatáshoz.

### <a name="prerequisites"></a>Előfeltételek

Ellenőrizze, hogy a következő előfeltételek teljesülnek hello:

- Informatikai szolgáltatás Management-összekötő telepítve.
További információ: [konfigurációs](log-analytics-itsmc-overview.md#configuration).
- a Service Manager webalkalmazás (webalkalmazás) hello telepítve és konfigurálva van. A webalkalmazás-információk [Itt](#create-and-deploy-service-manager-web-app-service).
- A hibrid kapcsolat létrehozása és konfigurálása. További információ: [konfigurálása hello hibrid kapcsolat](#configure-the-hybrid-connection).
- Támogatott verziók a Service Manager: 2012 R2 vagy a 2016.
- Felhasználói szerepkör: [speciális kezelő](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Kapcsolat létesítése

A következő eljárás tooconnect hello a System Center Service Manager-példány toohello IT Service Management-összekötő használatára:

1. Nyissa meg túl**OMS** >**beállítások** > **csatlakoztatott források**.
2. Válassza ki **ITSM összekötő** kattintson **új kapcsolat hozzáadása**.

    ![A Service manager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Hello adatokat hello a következő táblázatban leírtak szerint, és kattintson a **mentése** toocreate hello kapcsolat:

> [!NOTE]
> Ezek a paraméterek kötelezőek.

| **Mező** | **Leírás** |
| --- | --- |
| **Name (Név)**   | Írja be, hogy szeretné-e az informatikai szolgáltatás Management-összekötő hello tooconnect hello System Center Service Manager-példány nevét.  Használja a név később munkaelemek konfigurálja ezt a példányt / részletes naplóelemzési megtekintése. |
| **Válassza ki a kapcsolat típusa**   | Válassza ki **a System Center Service Manager**. |
| **URL-címe**   | Írja be a Service Manager webalkalmazás hello hello URL-CÍMÉT. A Service Manager webalkalmazás bővebb információk [Itt](#create-and-deploy-service-manager-web-app-service).
| **Ügyfél-azonosító**   | Írja be, hogy akkor jön létre (hello automatikus parancsfájl használatával) hello webalkalmazás hitelesítéséhez hello ügyfél-azonosító. Bővebb információk automatikus hello parancsfájl [itt.](log-analytics-itsmc-service-manager-script.md)|
| **Ügyfélkulcs**   | Típus hello ügyfélkulcs jön létre a azonosítóját.   |
| **Adatok szinkronizálási hatókör**   | Válassza ki, amelyet az informatikai szolgáltatás Management-összekötő hello keresztül toosync hello Service Manager munkaelemek.  A munkahelyi elemeket a rendszer importálta a Naplóelemzési. **Beállítások:** incidensek, Változáskérések.|
| **Szinkronizálja az adatokat** | Írja be a hello hány napra visszamenőleg, amelyet hello adatait. **Maximális**: 120 nap. |
| **Hozzon létre új konfigurációelemet ITSM megoldás** | Válassza ezt a beállítást, ha azt szeretné, hogy toocreate hello konfigurációs elemek hello ITSM termék. Kiválasztásakor, akkor a OMS hozza létre a hello érintett Konfigurációelemek, ahol a konfigurációs elemek (esetén nem létező CIs) hello támogatott ITSM rendszer. **Alapértelmezett**: le van tiltva. |

Ha sikeresen csatlakoztatva lett, és szinkronizálja azt:

- Kiválasztott munkaelemek a Service Manager OMS importálják **Naplóelemzési.** Megtekintheti a hello összefoglalása ezek munkaelemek a hello IT Service Management-összekötő csempére.

- Az OMS Szolgáltatáshoz létrehozhat OMS riasztásokat vagy a keresési napló, az incidensek a Service Manager-példány.

További információ: [OMS riasztások munkaelemek létrehozása ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) és [munkaelemek létrehozása ITSM OMS naplókból](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Létrehozhat és telepíthet a Service Manager web app service

tooconnect helyszíni Service Manager hello hello IT Service Management-összekötő OMS-ben a, a Microsoft hello GitHub a létrehozott egy Service Manager webalkalmazást.

hello ITSM webes alkalmazás a Service Manager, tooset hello a következő:

- **Telepítés hello webalkalmazás** – hello webalkalmazás telepítése hello tulajdonságainak beállítása és az Azure AD hitelesíti. Hello webalkalmazás hello segítségével telepíthet [parancsfájl automatikus](log-analytics-itsmc-service-manager-script.md) , hogy a Microsoft közzétett meg.
- **A hibrid kapcsolat hello konfigurálása** - [konfigurálja ezt a kapcsolatot](#configure-the-hybrid-connection), manuálisan.

#### <a name="deploy-hello-web-app"></a>Hello webalkalmazás telepítése
Használjon hello automatikus [parancsfájl](log-analytics-itsmc-service-manager-script.md) toodeploy webalkalmazás hello hello tulajdonságainak beállítása és az Azure AD hitelesíti.

Hello parancsprogrammal, adja meg a következő szükséges adatok hello:

- Azure-előfizetés részletei
- Az erőforráscsoport neve
- Hely
- A Service Manager-kiszolgálóadatok (kiszolgáló neve, tartomány, felhasználónév és jelszó)
- A webalkalmazás hely előtagja
- A Szolgáltatásbusz-Namespace.

hello parancsfájl létrehozza az hello néven megadott hello webalkalmazást (és néhány további karakterláncok toomake az egyedi). Hello generál **webes alkalmazás URL-címhez**, **ügyfél-azonosító** és **ügyfélkulcs**.

Mentés hello értékek használhatja őket az informatikai szolgáltatás Management-összekötő kapcsolatot hoz létre.

**Ellenőrizze a hello webalkalmazás telepítése**

1. Nyissa meg túl**Azure-portálon** > **erőforrások**.
2. Jelölje ki azt a webalkalmazást hello **beállítások** > **Alkalmazásbeállítások**.
3. Erősítse meg a hello Service Manager-példány hello hello parancsfájl hello elérhető alkalmazások központi telepítése során megadott hello információt.

### <a name="configure-hello-hybrid-connection"></a>A hibrid kapcsolat hello konfigurálása

A következő eljárás tooconfigure hello hibrid kapcsolat, amely a Service Manager-példány hello hello OMS IT Service Management-összekötő a hello használata.

1. A Service Manager Web app alkalmazásban hello alatt található **Azure-erőforrások**.
2. Kattintson a **beállítások** > **hálózati**.
3. A **hibrid kapcsolatok**, kattintson a **hibrid kapcsolati végpontok konfigurálása**.

    ![Hibrid kapcsolat hálózatkezelés](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. A hello **hibrid kapcsolatok** panelen kattintson a **adja hozzá a hibrid kapcsolat**.

    ![A hibrid kapcsolat hozzáadása](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. A hello **hibrid kapcsolatok hozzáadása** panelen kattintson a **hozzon létre új hibrid kapcsolat**.

    ![Új hibrid kapcsolat](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Írja be a következő értékek hello:

    - **A végpontnév**: Adjon meg egy nevet a hello új hibrid kapcsolat.
    -  **Végpont állomás**: hello Service Manager felügyeleti kiszolgáló teljes Tartományneve.
    - **Végponti Port**: írja be az 5724-es
    - **A Szolgáltatásbusz-névtér**: egy meglévő szolgáltatásbusz-névtér használatára, vagy hozzon létre egy újat.
    - **Hely**: Válasszon hello helyet.
    -  **Név**: Adjon meg egy nevet a toohello szolgáltatásbusz, ha létrehozása.

    ![Hibrid kapcsolati értékek](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Kattintson a **OK** tooclose hello **hibrid kapcsolat létrehozása** panel megnyitásához, és a start hello hibrid kapcsolat létrehozása.

    Hello hibrid kapcsolat létrehozása után hello panel alatt jelenik meg.

7. Hello hibrid kapcsolat létrehozása után válassza ki a hello kapcsolat, és kattintson a **hozzáadása a hibrid kapcsolat kijelölt**.

    ![Új hibrid kapcsolat](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-hello-listener-setup"></a>Hello figyelő telepítés konfigurálása

A következő eljárás tooconfigure hello figyelő beállításainak hello a hibrid kapcsolat hello használata.

1. A hello **hibrid kapcsolatok** panelen kattintson a **letöltési hello Csatlakozáskezelő** hello System Center Service Manager-példányt futtató számítógépen telepítse.

    Miután hello telepítés befejeződött, **Hybrid Connection Manager felhasználói felületén** beállítás érhető el a **Start** menü.

2. Kattintson a **Hybrid Connection Manager felhasználói felületén** , kérni fogja az Azure hitelesítő adatait.

3. Jelentkezzen be Azure hitelesítő adatait, és jelölje ki az előfizetését, ahol a hello hibrid kapcsolat létrehozása történt.

4. Kattintson a **Save** (Mentés) gombra.

A hibrid kapcsolat sikeresen csatlakoztatva van.

![a sikeres a hibrid kapcsolat](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Hello hibrid kapcsolat létrehozása után ellenőrizze, és hello kapcsolat tesztelése hello felkeresésével telepített Service Manager webalkalmazás. Győződjön meg arról hello kapcsolat sikeres előtt tooconnect toohello IT Service Management-összekötő az OMS Szolgáltatáshoz.

hello következő kép bemutatja a sikeres kapcsolat hello részleteit:

![A hibrid kapcsolat ellenőrzése](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-tooit-service-management-connector-in-oms"></a>A ServiceNow tooIT Service Management-összekötő az OMS csatlakozás

hello alábbi szakaszok nyújtanak részletes tájékoztatást tooconnect a ServiceNow termék toohello IT Service Management-összekötő az OMS Szolgáltatáshoz.

### <a name="prerequisites"></a>Előfeltételek

Ellenőrizze, hogy a következő előfeltételek teljesülnek hello:

- Informatikai szolgáltatás Management-összekötő telepítve. További információ: [konfigurációs.](log-analytics-itsmc-overview.md#configuration)
- A ServiceNow verzió – Fudzsi, Geneva, Helsinki támogatott.

A ServiceNow rendszergazdák kell tennie a ServiceNow példánya a következő hello:
- Ügyfél-azonosító és a titkos ügyfélkulcs hello ServiceNow termék létrehozása. Hogyan toogenerate ügyfél-azonosító és a titkos kulcsot: kapcsolatos [OAuth telepítését](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Telepítse a Microsoft OMS-integráció (ServiceNow alkalmazás) felhasználói alkalmazás hello. [További információk](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Hello felhasználói alkalmazás telepített integrációs felhasználói szerepkört létrehozni. Hogyan toocreate hello integrációs felhasználói szerepköre információk [Itt](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Kapcsolat létesítése**

A következő eljárás toocreate ServiceNow kapcsolatot hello használata:

1. Nyissa meg túl**OMS** > **beállítások** > **csatlakoztatott források**.
2. Válassza ki **ITSM összekötő** kattintson **új kapcsolat hozzáadása**.

    ![A ServiceNow kapcsolat](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Hello adatokat hello a következő táblázatban leírtak szerint, és kattintson a **mentése** toocreate hello kapcsolat:

> [!NOTE]
> Ezek a paraméterek kötelezőek.

| **Mező** | **Leírás** |
| --- | --- |
| **Name (Név)**   | Írja be, hogy szeretné-e az informatikai szolgáltatás Management-összekötő hello tooconnect hello ServiceNow-példány nevét.  Ez a név később az OMS használni, amikor a munkaelemek konfigurálja a ITSM / részletes naplóelemzési megtekintése. |
| **Válassza ki a kapcsolat típusa**   | Válassza ki **ServiceNow**. |
| **Felhasználónév**   | Írja be a hello integrációs felhasználónevet, hello ServiceNow app toosupport hello kapcsolat toohello IT Service Management-összekötő az első létrehozott. További információ: [létrehozása ServiceNow alkalmazás felhasználói szerepkör](#create-integration-user-role-in-servicenow-app).|
| **Jelszó**   | Írja be ezt a felhasználónevet tartozó hello jelszót. **Megjegyzés:**: felhasználónév és jelszó generálásához. csak a hitelesítési tokenek használatát, és nem tárolja el bárhonnan hello OMS szolgáltatáshoz.  |
| **URL-címe**   | Írja be a hello ServiceNow példány, amelyet az tooconnect tooIT Management-összekötő hello URL-CÍMÉT. |
| **Ügyfél-azonosító**   | Írja be a kívánt toouse OAuth2-hitelesítést, amelyet korábban létrehozott hello ügyfél-azonosító.  További információ az ügyfél-azonosító és a titkos kulcs létrehozása: [OAuth telepítését](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Ügyfélkulcs**   | Típus hello ügyfélkulcs jön létre a azonosítóját.   |
| **Adatok szinkronizálási hatókör**   | Válassza ki a megjeleníteni kívánt toosync tooOMS hello IT Service Management-összekötő segítségével hello ServiceNow munkaelemek.  kijelölt hello értéket a rendszer importálta a naplóelemzési.   **Beállítások:** incidensek és Változáskérések.|
| **Szinkronizálja az adatokat** | Írja be a hello hány napra visszamenőleg, amelyet hello adatait. **Maximális**: 120 nap. |
| **Hozzon létre új konfigurációelemet ITSM megoldás** | Válassza ezt a beállítást, ha azt szeretné, hogy toocreate hello konfigurációs elemek hello ITSM termék. Kiválasztásakor, akkor a OMS hozza létre a hello érintett Konfigurációelemek, ahol a konfigurációs elemek (esetén nem létező CIs) hello támogatott ITSM rendszer. **Alapértelmezett**: le van tiltva. |


Ha sikeresen csatlakoztatva lett, és szinkronizálja azt:

- Kijelölt elemek ServiceNow kapcsolatról a rendszer importálta OMS Naplóelemzési munkahelyi.  Megtekintheti a hello összefoglalása ezek munkaelemek a hello IT Service Management-összekötő csempére.
- A ServiceNow példány OMS riasztásokat vagy a napló keresésből incidensek, a riasztások és az események hozhat létre.  


További információ: [OMS riasztások munkaelemek létrehozása ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) és [munkaelemek létrehozása ITSM OMS naplókból](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>A ServiceNow app integrációs felhasználói szerepkör létrehozása

Felhasználói hello a következő eljárást:

1.  Látogasson el a hello [ServiceNow tároló](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) és hello telepítése **felhasználói alkalmazás a ServiceNow és a Microsoft OMS-integráció** azokat a ServiceNow példányát.
2.  A telepítés után nyissa meg a bal oldali navigációs sáv hello ServiceNow példány, a Keresés és a jelölje be a Microsoft OMS integráló hello.  
3.  Kattintson a **telepítési ellenőrzőlista**.

    hello állapot jelenik meg **nem hajtható végre,** Ha hello a felhasználói szerepkör még toobe létrehozni.

4.  Hello szöveges mezők, ezután túl**integráció a felhasználó létrehozása**, adjon meg felhasználónevet hello hello felhasználó csatlakoztatható toohello OMS IT Service Management-összekötő.
5.  Adja meg a hello jelszót ehhez a felhasználóhoz, majd kattintson **OK**.  

>[!NOTE]

> Ezen hitelesítő adatok toomake hello ServiceNow kapcsolat használata az OMS Szolgáltatáshoz.

az újonnan létrehozott felhasználó hello jelenik meg, amely hello alapértelmezett szerepkörrel.

Alapértelmezett szerepkörök:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   ITIL
-   template_editor
-   view_changer

Hello felhasználó sikeres létrehozását követően hello állapotának **ellenőrizze telepítési ellenőrzőlista** tooCompleted listaelem hello részletei hello felhasználói szerepkör létre hello alkalmazáshoz helyezi.

> [!NOTE]

> egy felhasználó toocreate tooallow **riasztások** és **események** a ServiceNow az OMS Szolgáltatáshoz:

> - Győződjön meg arról, a ServiceNow példányán hello esemény modul telepítve van.

> - Adja hozzá a következő szerepkörök toohello integrációs felhasználó hello:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-tooit-service-management-connector-in-oms"></a>Csatlakozás a Service Management-összekötő az OMS Provance tooIT

hello alábbi szakaszok nyújtanak részletes tájékoztatást tooconnect a Provance termék toohello IT Service Management-összekötő az OMS Szolgáltatáshoz.

### <a name="prerequisites"></a>Előfeltételek

Ellenőrizze, hogy a következő előfeltételek teljesülnek hello:

- Informatikai szolgáltatás Management-összekötő telepítve. További információ: [konfigurációs](log-analytics-itsmc-overview.md#configuration).
- Provance App kell regisztrálnia az Azure AD - és ügyfél-azonosító szeretné elérhetővé tenni. Részletes információkért lásd: [hogyan tooconfigure active directory-hitelesítés](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
- Felhasználói szerepkör: rendszergazda.

### <a name="connection-procedure"></a>Kapcsolat létesítése

A következő eljárás toocreate Provance kapcsolatot hello használata:

1. Nyissa meg túl**OMS** > **beállítások** > **csatlakoztatott források**.
2. Válassza ki **ITSM összekötő** kattintson **új kapcsolat hozzáadása**.  

    ![Provance kapcsolat](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Hello adatokat hello a következő táblázatban leírtak szerint, és kattintson a **mentése** toocreate hello kapcsolat.

> [!NOTE]
> Ezek a paraméterek kötelezőek.

| **Mező** | **Leírás** |
| --- | --- |
| **Name (Név)**   | Írja be, hogy szeretné-e az informatikai szolgáltatás Management-összekötő hello tooconnect hello Provance példány nevét.  Ez a név később az OMS használni, amikor a munkaelemek konfigurálja a ITSM / részletes naplóelemzési megtekintése. |
| **Válassza ki a kapcsolat típusa**   | Válassza ki **Provance**. |
| **Felhasználónév**   | Írja be hello felhasználónevét, amely toohello IT Service Management-összekötő képes kapcsolódni.    |
| **Jelszó**   | Írja be ezt a felhasználónevet tartozó hello jelszót. **Megjegyzés:** felhasználónév és jelszó generálásához. csak a hitelesítési tokenek használatát, és nem tárolja el bárhonnan hello OMS szolgáltatáshoz. _|
| **URL-címe**   | Írja be a megjeleníteni kívánt tooconnect tooIT Management-összekötő Provance példányát hello URL-CÍMÉT. |
| **Ügyfél-azonosító**   | Hello ügyfél-Azonosítót adja meg ezt a kapcsolatot, a Provance példányt létrehozó hitelesítéséhez.  További információ az ügyfél-azonosító, lásd: [hogyan tooconfigure active directory-hitelesítés](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Adatok szinkronizálási hatókör**   | Válassza ki a megjeleníteni kívánt toosync tooOMS hello IT Service Management-összekötő segítségével hello Provance munkaelemek.  A munkahelyi elemeket a rendszer importálta a naplóelemzési.   **Beállítások:** incidensek, Változáskérések.|
| **Szinkronizálja az adatokat** | Írja be a hello hány napra visszamenőleg, amelyet hello adatait. **Maximális**: 120 nap. |
| **Hozzon létre új konfigurációelemet ITSM megoldás** | Válassza ezt a beállítást, ha azt szeretné, hogy toocreate hello konfigurációs elemek hello ITSM termék. Kiválasztásakor, akkor a OMS hozza létre a hello érintett Konfigurációelemek, ahol a konfigurációs elemek (esetén nem létező CIs) hello támogatott ITSM rendszer. **Alapértelmezett**: le van tiltva.|

Ha sikeresen csatlakoztatva lett, és szinkronizálja azt:

- Kiválasztott munkaelemek Provance kapcsolatról a rendszer importálta OMS **Naplóelemzési.**  Megtekintheti a hello összefoglalása ezek munkaelemek a hello IT Service Management-összekötő csempére.
- Létrehozhat az incidensek és események OMS riasztásokat vagy a napló keresési a Provance példányában.

További információ: [OMS riasztások munkaelemek létrehozása ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) és [munkaelemek létrehozása ITSM OMS naplókból](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-tooit-service-management-connector-in-oms"></a>Csatlakozás a Service Management-összekötő az OMS Cherwell tooIT

hello alábbi szakaszok nyújtanak részletes tájékoztatást tooconnect a Cherwell termék toohello IT Service Management-összekötő az OMS Szolgáltatáshoz.

### <a name="prerequisites"></a>Előfeltételek

Ellenőrizze, hogy a következő előfeltételek teljesülnek hello:

- Informatikai szolgáltatás Management-összekötő telepítve. További információ: [konfigurációs](log-analytics-itsmc-overview.md#configuration).
- Létrehozott ügyfél-azonosító. További információ: [készítése az ügyfél-azonosító Cherwell](#generate-client-id-for-cherwell).
- Felhasználói szerepkör: rendszergazda.

### <a name="connection-procedure"></a>Kapcsolat létesítése

A következő eljárás toocreate Cherwell kapcsolatot hello használata:

1. Nyissa meg túl**OMS** >  **beállítások** > **csatlakoztatott források**.
2. Válassza ki **ITSM összekötő** kattintson **új kapcsolat hozzáadása**.  

    ![Cherwell felhasználói azonosító](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Hello adatokat hello a következő táblázatban leírtak szerint, és kattintson a **mentése** toocreate hello kapcsolat.

> [!NOTE]
> Ezek a paraméterek kötelezőek.

| **Mező** | **Leírás** |
| --- | --- |
| **Name (Név)**   | Írja be, amelyet az informatikai szolgáltatás Management-összekötő tooconnect toohello hello Cherwell példány nevét.  Ez a név később az OMS használni, amikor a munkaelemek konfigurálja a ITSM / részletes naplóelemzési megtekintése. |
| **Válassza ki a kapcsolat típusa**   | Válassza ki **Cherwell.** |
| **Felhasználónév**   | Írja be a hello Cherwell felhasználónevet toohello IT Service Management-összekötő csatlakoztatható. |
| **Jelszó**   | Írja be ezt a felhasználónevet tartozó hello jelszót. **Megjegyzés:** felhasználónév és jelszó generálásához. csak a hitelesítési tokenek használatát, és nem tárolja el bárhonnan hello OMS szolgáltatáshoz.|
| **URL-címe**   | Írja be a megjeleníteni kívánt tooconnect tooIT Management-összekötő Cherwell példányát hello URL-CÍMÉT. |
| **Ügyfél-azonosító**   | Hello ügyfél-Azonosítót adja meg ezt a kapcsolatot, a Cherwell példányt létrehozó hitelesítéséhez.   |
| **Adatok szinkronizálási hatókör**   | Válassza ki a megjeleníteni kívánt hello IT Service Management-összekötő segítségével toosync hello Cherwell munkaelemek.  A munkahelyi elemeket a rendszer importálta a naplóelemzési.   **Beállítások:** incidensek, Változáskérések. |
| **Szinkronizálja az adatokat** | Írja be a hello hány napra visszamenőleg, amelyet hello adatait. **Maximális**: 120 nap. |
| **Hozzon létre új konfigurációelemet ITSM megoldás** | Válassza ezt a beállítást, ha azt szeretné, hogy toocreate hello konfigurációs elemek hello ITSM termék. Kiválasztásakor, akkor a OMS hozza létre a hello érintett Konfigurációelemek, ahol a konfigurációs elemek (esetén nem létező CIs) hello támogatott ITSM rendszer. **Alapértelmezett**: le van tiltva. |

Ha sikeresen csatlakoztatva lett, és szinkronizálja azt:

- Kijelölt munkahelyi Cherwell csolat szereplő elemeket a rendszer importálta OMS szolgáltatáshoz. Megtekintheti a hello összefoglalása ezek munkaelemek a hello IT Service Management-összekötő csempére.
- Ebben a példában az OMS Szolgáltatáshoz Cherwell incidensek és események hozhat létre. További információ: létrehozás ITSM munkaelemek OMS-riasztások és létrehozása ITSM munkaelemek OMS naplókból.

További információ: [OMS riasztások munkaelemek létrehozása ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) és [munkaelemek létrehozása ITSM OMS naplókból](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>Ügyfél-azonosító Cherwell létrehozása

toogenerate hello ügyfél Cherwell, a következő eljárás használható hello azonosítója/kulcsa:

1. Jelentkezzen be tooyour Cherwell példány rendszergazdával.
2. Kattintson a **biztonsági** > **szerkesztése a REST API-t ügyfélbeállítások**.
3. Válassza ki **hozzon létre új ügyfél** > **ügyfélkulcs**.

    ![Cherwell felhasználói azonosító](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Következő lépések
 - [Az OMS-értesítések ITSM munkaelemek létrehozása](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [Az OMS-naplók ITSM munkaelemek létrehozása](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [A kapcsolat a naplóelemzési megtekintése](log-analytics-itsmc-overview.md#using-the-solution)
