---
title: egy helyi SQL Server az Azure Machine Learning aaaUse |} Microsoft Docs
description: "Egy helyi SQL Server adatbázis tooperform advanced analytics adatait használják az Azure Machine Learning segítségével."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: garye;krishnan
ms.openlocfilehash: c0e9908e296b97b31611ef0192744a59073acd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Bővített analitika az Azure Machine Learning használatával egy helyszíni SQL Server-adatbázisból származó adatokkal

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Gyakran a helyszíni adatokkal dolgozni vállalatok hello méretezési tootake kihasználása és a gépi tanulási a munkaterhelések hello-felhő agilitásának szeretné. De azok nem szeretnénk toodisrupt az aktuális üzleti folyamatokat és munkafolyamatok azáltal, hogy azok a helyszíni adatok toohello felhő. Az Azure Machine Learning mostantól támogatja az adatok olvasása a helyszíni SQL Server adatbázis, majd képzési és ezeket az adatokat egy modell pontozása. Már nincs toomanually másolása és szinkronizálási hello adatok hello felhő és a helyszíni kiszolgáló között. Ehelyett hello **és adatokat importálhat** modul az Azure Machine Learning Studióban elolvashatja a helyszíni SQL Server adatbázis-ről a képzés és a feladatok pontozási.

Ez a cikk áttekintést hogyan tooingress a helyszíni SQL server-adatok az Azure Machine Learning. Feltételezi, hogy ismeri az Azure Machine Learning fogalmak munkaterületek, modulok, adatkészleteket, kísérleteket, például *stb*.

> [!NOTE]
> Ez a funkció nem érhető el a munkaterületek. További információ a Machine Learning díjszabás és rétegek: [Azure Machine Learning díjszabás](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="install-hello-microsoft-data-management-gateway"></a>Telepítse a Microsoft adatkezelési átjáró hello
az Azure Machine Learning a helyszíni SQL Server adatbázis tooaccess, toodownload és szükséges telepítési hello Microsoft adatkezelési átjáró. Hello gateway-kapcsolatot a Machine Learning Studióban konfigurálásakor hello lehetőség arra, hogy töltse le és telepítse az átjárót hello hello rendelkezik **letöltés, és regisztrálja az adatátjárót** párbeszédpanel az alábbiakban.

Is elvégezheti a telepítést az adatkezelési átjáró hello időben letöltése és hello MSI-telepítő csomagot futtatja hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).
Válassza ki a hello legújabb verziót, 32 bites vagy 64 bites, a számítógép megfelelő. hello MSI is használt tooupgrade meglévő az adatkezelési átjáró toohello legújabb verziója, az összes beállítás megőrzi.

hello átjáró a következő előfeltételek hello rendelkezik:

* hello támogatott Windows operációs rendszer verziója a Windows 7, Windows 8 vagy 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 és a Windows Server 2012 R2 rendszer.
* hello ajánlott hello átjáró gép konfigurációja legalább 2 GHz, 4 mag, 8GB RAM és 80GB lemezterület.
* Ha a hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol. Ezért konfigurálása egy megfelelő energiaséma számítógépen hello hello-átjáró telepítése előtt. Ha hello gép konfigurált toohibernate, hello átjáró telepítésének üzenetet jelenít meg.
* Egy adott gyakorisággal másolási tevékenység során következik be, mert a hello erőforrás-használat (Processzor, memória) hello gépen is követi hello azonos mintát csúcs és üresjárati idő. Erőforrás-használat is függ, erősen hello áthelyezett adatok mennyiségét. Több másolási feladat van folyamatban, akkor lesz megfigyelheti csúcsidőben menni Erőforrás kihasználtsága. Míg a fent felsorolt minimális konfiguráció hello technikailag elegendő, érdemes lehet toohave biztosítson több erőforrást, mint a minimális konfigurációs hello konfiguráció attól függően, hogy a meghatározott típusú az adatátvitelt jelölik.

Vegye figyelembe a hello következőket telepítésekor és az adatkezelési átjáró használatának:

* Az adatkezelési átjáró csak egy példánya is telepíthető egy egyedi számítógép.
* Csak egyetlen átjáró használható több helyszíni adatforráshoz.
* Csatlakozhat a különböző számítógépeken toohello több átjáró ugyanazon a helyszíni adatforráshoz.
* Egyszerre csak egy munkaterület átjárók konfigurálása. Jelenleg átjárók munkaterületek között is megoszthatja.
* Több átjáró egyetlen munkaterület konfigurálhatja. Előfordulhat például egy átjáró, amely csatlakoztatott tooyour teszt adatforrások fejlesztési és éles átjárót toouse azt szeretné, ha már készen áll a toooperationalize.
* hello átjáró nem kell azonos számítógépre hello adatforrásként hello a toobe. De szorosabb toohello marad adatforrás csökkenti a hello idő hello átjáró tooconnect toohello az adatforrásra vonatkozóan. Azt javasoljuk, hogy olyan számítógépen, amelyen eltér a hello egy állomások hello a helyszíni adatforráshoz, hogy hello átjáró és az adatforrás nem "versenyeznek" az erőforrások hello átjáró telepítése.
* Ha már van egy átjáró a Power bi-ban vagy az Azure Data Factory forgatókönyvek szolgáltató számítógépre telepítve, telepítsen egy különálló átjáróra az Azure Machine Learning egy másik számítógépen.

  > [!NOTE]
  > A hello nem futtatható az adatkezelési átjáró és a Power BI-átjáró ugyanazon a számítógépen.
  >
  >
* Toouse hello az adatkezelési átjáró kell az Azure Machine Learning még akkor is, ha az Azure ExpressRoute az egyéb adatokat használ. Kezelje az adatforrás egy helyszíni adatforrás (tűzfal mögött van), még akkor is ExpressRoute használatakor. Hello az adatkezelési átjáró tooestablish Machine Learning és a hello adatforrás közötti kapcsolat használata.

Hello cikkben található részletes információkat a telepítési előfeltételek, a telepítési lépések és a hibaelhárítási tippekért [az adatkezelési átjáró](../data-factory/data-factory-data-management-gateway.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Érkező adatokat az Azure Machine Learning a helyszíni SQL Server-adatbázisból
Ebben a bemutatóban adatkezelési átjárót beállítása az Azure Machine Learning-munkaterület, azt konfigurálása, és olvassa el az adatok egy helyi SQL Server-adatbázisból.

> [!TIP]
> Mielőtt elkezdené, tiltsa le a böngésző előugróablak-blokkoló a `studio.azureml.net`. Google Chrome böngésző hello használata, töltse le és telepíteni az egyik hello több beépülő modulok érhető el a Google Chrome webes tároló [kattintson egyszer bővítmény](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
>

### <a name="step-1-create-a-gateway"></a>1. lépés: Az átjáró létrehozása
hello első lépés toocreate, és a helyszíni SQL-adatbázis hello átjáró tooaccess beállítása.

1. Jelentkezzen be túl[Azure Machine Learning Studio](https://studio.azureml.net/Home/) és a kívánt toowork válassza hello munkaterületen.
2. Kattintson a hello **beállítások** hello paneljét maradt, és kattintson a hello **DATA GATEWAYS** hello felső lapon.
3. Kattintson a **az új ADATÁTJÁRÓ** üdvözlő képernyőt hello alján.

    ![Az új adatátjáró](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. A hello **az új adatátjáró** párbeszédpanelen adja meg a hello **átjáró neve** , és opcionálisan adja hozzá egy **leírása**. Kattintson a hello alsó jobb sarkában toogo toohello következő lépésben hello konfiguráció hello nyílra.

    ![Adja meg az átjáró nevét és leírását](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. A hello töltse le, és regisztrálja az adatok átjáró párbeszédpanelen másolási hello ÁTJÁRÓ regisztrációs kulcs toohello vágólapra.

    ![Töltse le, és regisztrálja az adatátjárót](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Ha még nem letöltötte és telepítette a Microsoft adatkezelési átjáró hello, majd **letöltési az adatkezelési átjáró**. Ez vesz toothe Microsoft Download Center hello az átjáró verziójának kiválasztásához meg kell, töltse le és telepítse. Részletes információkat a telepítési előfeltételek, a telepítési lépések és a hibaelhárítási tippekért hello cikk hello kezdete szakaszaiban található [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjárófelhőközött](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).
7. Hello átjáró telepítése után az adatkezelési átjáró konfigurációkezelőjének hello megnyitja és hello **Register átjáró** párbeszédpanel jelenik meg. Beillesztés hello **átjáró regisztrációs kulcs** toohello vágólapra, majd kattintson másolt **regisztrálása**.
8. Ha már van egy telepített átjárót, futtassa az adatkezelési átjáró konfigurációkezelőjének hello. Kattintson a **kulcsváltás**, illessze be a **átjáró regisztrációs kulcs** toohello vágólapra másolt hello előző lépést, és kattintson a **OK**.
9. Ha hello telepítés befejeződött, hello **Register átjáró** a Microsoft adatkezelési átjáró konfigurációkezelőjének párbeszédpanel jelenik meg. Illessze be a hello hello vágólapra az előző lépésben másolt ÁTJÁRÓ regisztrációs kulcsot, és kattintson a **regisztrálása**.

    ![Átjáró regisztrálása](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. hello átjáró konfigurálása nem fejeződött be a következő értékek hello a hello beállításakor **Home** lapon a Microsoft adatkezelési átjáró konfigurációkezelőjének:

    * **Átjáró neve** és **példánynév** hello átjáró toohello neve van beállítva.
    * **Regisztrációs** értéke túl**regisztrált**.
    * **Állapot** értéke túl**elindítva**.
    * hello állapotsorban hello alsó megjeleníti **csatlakoztatva az adatkezelési átjáró Felhőszolgáltatáshoz tooData** mellett zöld pipa jelzi.

      ![Felügyeleti átjáró adatkezelő](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Az Azure Machine Learning Studio is frissül a hello regisztráció sikeres esetén.

    ![Átjáró regisztrálása sikeres](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. A hello **töltse le és adatátjáró regisztrálására** párbeszédpanel, kattintson a pipa jelre toocomplete hello beállítása. Hello **beállítások** lapon megjelenik az átjáró állapotának "Elérhető". Hello jobb oldali ablaktáblában állapota és más hasznos információkat találhat.

    ![Átjáró beállításai](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-status.png)
12. A Microsoft adatkezelési átjáró konfigurációkezelőjének hello kapcsoló toohello **tanúsítvány** ezen a lapon megadott külön-külön hello tanúsítvány hello helyszíni adattár megadott használt tooencrypt/visszafejtési hitelesítő adatai hello portálon. Ez a tanúsítvány az hello alapértelmezett tanúsítvány. A Microsoft azt javasolja, hogy ez a tanúsítvány felügyeleti rendszer biztonsági mentést saját tooyour a tanúsítvány módosítása. Kattintson a **módosítás** toouse saját tanúsítvány helyette.

    ![Átjáró tanúsítvány módosítása](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (választható) Ha azt szeretné, hogy tooenable részletes naplózás hello átjáró elhárítása érdekében, a Microsoft adatkezelési átjáró konfigurációkezelőjének hello kapcsoló toothe **diagnosztika** fülre és ellenőrizze a hello **engedélyezése részletes naplózás hibaelhárítási célból** lehetőséget. hello naplóinformációk található hello a Windows Eseménynapló hello **alkalmazási és Szolgáltatásnaplójában**  - &gt; **az adatkezelési átjáró** csomópont. Is használhatja a hello **diagnosztika** lapon tootest hello kapcsolat tooan helyszíni adatforrás hello átjáró használatával.

    ![Részletes naplózás engedélyezése](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Ezzel befejezte a hello átjáró telepítési folyamat az Azure Machine Learning.
Ön éppen most már készen áll a toouse a helyszíni adatokat.

Hozzon létre, és állítsa be az egyes munkaterületeken Studio több átjáró. Például előfordulhat, hogy egy átjáró, amelyet tooconnect tooyour teszt adatforrások és a fejlesztői, egy másik átjáró éles adatforrások. Az Azure Machine Learning lehetővé teszi a rugalmasság tooset be több átjáró attól függően, a vállalati környezetben. Jelenleg egy átjáró munkaterületek között nem osztható meg, és csak egy átjáró telepíthető egy számítógépen. További információkért lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-hello-gateway-tooread-data-from-an-on-premises-data-source"></a>2. lépés: A használandó hello átjáró tooread adatok egy helyszíni adatforrás
Hello átjáró beállítása után hozzáadhat egy **és adatokat importálhat** modult egy, a bemeneti hello a helyszíni SQL Server-adatbázis hello adatait.

1. A Machine Learning Studióban, válassza ki a hello **kísérletek** lapra, majd **+ új** hello bal alsó sarkában, és válassza ki **üres kísérlet** (vagy jelöljön ki egy több minta kísérletek érhető el).
2. Keresse meg, és húzza hello **és adatokat importálhat** modul toohello kísérlet vászonra.
3. Kattintson a **Mentés másként** hello vászon alatt. Adja meg "Azure Machine Learning a helyszíni SQL Server oktatóanyag" hello kísérlet nevét, a munkaterület kiválasztása, és kattintson a hello **OK** pipára.

   ![Kísérlet mentése új néven](media/machine-learning-use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Hello kattintson **és adatokat importálhat** modul tooselect, ezt a a **tulajdonságok** ablaktábla toohello jog a vásznon a hello, jelölje ki "A helyszíni SQL adatbázis" hello **adatforrás** a legördülő listában.
5. Jelölje be hello **adatátjáró** telepítve és regisztrálva. "(Hozzáadása az új adatátjáró...)" kiválasztásával állíthat be egy másik átjárót.

   ![Válassza ki az adatok importálása modul adatátjáró](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Adja meg az hello SQL **adatbázis-kiszolgáló neve** és **adatbázisnév**, együtt hello SQL **adatbázis-lekérdezés** tooexecute szeretné.
7. Kattintson a **adja meg az értékeket** alatt **felhasználónevet és jelszót** és az adatbázis hitelesítő adatait. Használhatja az integrált Windows-hitelesítést vagy SQL Server-hitelesítés a helyszíni SQL Server konfigurációtól függően.

   ![Adja meg az adatbázis hitelesítő adatai](media/machine-learning-use-data-from-an-on-premises-sql-server/database-credentials.png)

   "értékek a szükséges" módosításokat túl "értéket állítsa" zöld pipa jelzi a üdvözlőüzenetére. Csak akkor kell tooenter hello hitelesítő adatok egyszer kivéve, ha az adatbázis-információ hello vagy jelszavának megváltozását. Az Azure Machine Learning hello átjáró tooencrypt hello hitelesítő adatok felhőben hello telepítésekor megadott hello tanúsítványt használ. Azure soha nem tárolja a helyszíni hitelesítő adatok titkosítás nélkül.

   ![Adattulajdonságainak modul importálása](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Kattintson a **futtatása** toorun hello kísérlet.

Hello kísérlet befejezése után történik, ha jelenítheti meg hello kimeneti portjára hello kattintva hello adatbázisból importált hello adatok **és adatokat importálhat** modult, majd válassza **Visualize**.

Miután befejezte a kísérletben fejleszt, telepítheti és üzembe helyezheti modelljét. Kötegelt végrehajtási szolgáltatás, konfigurálva hello hello a helyszíni SQL Server adatbázis használatával hello **és adatokat importálhat** modul beolvassa és pontozó használni kell. Hello kérelem-válasz szolgáltatás használhatja a pontozási a helyszíni adatok, a Microsoft azt javasolja, használja a [Excel-bővítmény](machine-learning-excel-add-in-for-web-services.md) helyette. Jelenleg tooan írása a helyszíni SQL Server-adatbázis keresztül **adatok exportálása** nem támogatott, vagy a kísérleti vagy a közzétett webes szolgáltatások.
