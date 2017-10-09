---
title: "az Azure Data Factory adatmozgás aaaSecurity szempontjai |} Microsoft Docs"
description: "További információk a biztonságossá tétele az Azure Data Factory adatátvitelt jelölik."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 4bfce8884df14ad5b94e28ad3dfcf7025e2130a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Az Azure Data Factory - adatátvitelt jelölik a kapcsolódó biztonsági szempontok
## <a name="introduction"></a>Bevezetés
Ez a cikk ismerteti az alapvető biztonsági infrastruktúra adatátviteli szolgáltatások az Azure Data Factory használó toosecure adatait. Az Azure Data Factory-kezelési források az Azure biztonsági infrastruktúra épül, és használja az Azure által kínált minden lehetséges biztonsági intézkedéseket.

A Data Factory-megoldásokkal egy vagy több [adatfolyamatot](data-factory-create-pipelines.md) is létrehozhat. A folyamatok olyan tevékenységek logikus csoportosításai, amelyek együttesen vesznek részt egy feladat végrehajtásában. Ezek a folyamatok várakozó hello régió, ahol a hello adat-előállító létrehozása történt. 

Annak ellenére, hogy a Data Factory érhető el csak **USA nyugati régiója**, **USA keleti régiója**, és **Észak-Európa** régiókban, hello adatátviteli szolgáltatás érhető el [globálisan a több régiók](data-factory-data-movement-activities.md#global). Adat-előállító szolgáltatás biztosítja, hogy adatokat nem egy földrajzi terület marad / régió kivéve, ha explicit módon utasíthatja hello szolgáltatás toouse egy másik régióban Ha hello adatátviteli szolgáltatás nincs még telepítve toothat régió. 

Az Azure Data Factory maga nem tárol kivéve a társított szolgáltatás hitelesítő adatait felhőalapú adattároló, tanúsítványok használata titkosított adatokat. Lehetővé teszi az adatvezérelt munkafolyamatok közötti tooorchestrate Mozgás létrehozása [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) és az adatok feldolgozása [szolgáltatások számítási](data-factory-compute-linked-services.md) más régiókban vagy egy helyszíni környezet. Azt is lehetővé teszi túl[felügyeletéhez és kezeléséhez munkafolyamatok](data-factory-monitor-manage-pipelines.md) mind programozott és felhasználói felülete mechanizmust.

Azure Data Factory használatával adatmozgás lett **hitelesített** esetében:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA CSILLAG](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Ha Ön Azure megfelelőségét, és hogyan Azure biztosítja a saját infrastruktúra iránt érdeklődik, keresse fel a hello [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

Ez a cikk azt a következő két adatok mozgása forgatókönyvek hello biztonsági szempontok tekintse át: 

- **Felhő használata estén**-ebben a forgatókönyvben a forrás- és is nyilvánosan elérhető az interneten keresztül. Ezek közé tartozik a felügyelt tárolási felhőszolgáltatások pl. Azure Storage Azure SQL Data Warehouse, az Azure SQL Database, az Azure Data Lake Store, Amazon S3, Amazon Redshift, például a Salesforce szolgáltatott szoftver szolgáltatásokat, és a webes protokollok-pl.: FTP- és OData. A támogatott adatforrások teljes listáját megtalálhatja [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Hibrid forgatókönyvek**– ebben a forgatókönyvben a cél- és tűzfal mögött található, vagy egy helyszíni vállalati hálózati vagy hello adatok belül tároló egy magánhálózaton / virtuális hálózatot (leggyakrabban hello forrás), és nincs nyilvánosan elérhető . Adatbázis-kiszolgálóin futó virtuális gépek is alá ebben a forgatókönyvben.

## <a name="cloud-scenarios"></a>Felhő forgatókönyvek
###<a name="securing-data-store-credentials"></a>Biztonságossá tétele adatok adattárolóhoz használandó hitelesítő adatok
Az Azure Data Factory védi az adatokat adattárolóhoz használandó hitelesítő adatok szerint **titkosított** őket a **tanúsítványokat a Microsoft által felügyelt**. Ezek a tanúsítványok legyenek-e elforgatva minden **kétéves** (amely tartalmazza a tanúsítvány megújítása és a hitelesítő adatok áttelepítését). A titkosított hitelesítő adatokat biztonságosan vannak tárolva egy **Azure Storage Azure Data Factory szolgáltatások által kezelt**. Azure Storage biztonsággal kapcsolatos további információkért tekintse meg a [Azure Storage biztonsági áttekintése](../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Adattitkosítás átvitel közben
Ha hello felhőalapú adattároló támogatja a HTTPS vagy a TLS, adat-előállítóban adatátviteli szolgáltatások közötti minden adatátvitel és felhőalapú adattároló HTTPS- vagy TLS biztonságos csatornán keresztül.

> [!NOTE]
> Az összes kapcsolat túl**Azure SQL Database** és **Azure SQL Data Warehouse** mindig titkosítás (SSL/TLS) közben az átvitel közben tooand hello adatbázisból. Egy folyamatot, egy JSON-szerkesztővel készítése, során adja hozzá a hello **titkosítási** tulajdonság, majd állítsa be túl**igaz** a hello **kapcsolati karakterlánc**. Amikor az hello [másolása varázsló](data-factory-azure-copy-wizard.md), hello varázsló alapértelmezés szerint állítja ezt a tulajdonságot. A **Azure Storage**, használhat **HTTPS** hello kapcsolati karakterláncban.

### <a name="data-encryption-at-rest"></a>Adat-titkosítás inaktív állapotban
Egyes adatokat inaktív adatok titkosítása támogatási tárolja. Javasoljuk, hogy engedélyezze ezeket adattárolókhoz adatok titkosítási mechanizmus. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transzparens Data Encryption (TDE) az Azure SQL Data Warehouse elősegíti a valós idejű titkosítási és visszafejtési az adatok aktívan elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem. Ez a viselkedés a transzparens toohello ügyfél. További információkért lásd: [az SQL Data Warehouse adatbázis védelme](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Az Azure SQL adatbázis is támogatja a transzparens adattitkosítás (TDE), amely segít a valós idejű titkosítási és visszafejtési hello adatok anélkül, hogy a módosítások toohello alkalmazás elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem. Ez a viselkedés a transzparens toohello ügyfél. További információkért lásd: [átlátható adattitkosítást az Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store is hello fiókban tárolt adatok titkosítását biztosítja. Ha engedélyezve van, a Data Lake store automatikusan megőrzése előtt titkosítja az adatokat, és lekérése, így átlátható toohello ügyfél hello adatok elérése előtt visszafejti. További információkért lásd: [az Azure Data Lake Store biztonsági](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Az Azure Blob Storage és az Azure Table Storage
Az Azure Blob Storage és az Azure Table storage támogatja a Storage szolgáltatás titkosítási (SSE), amely automatikusan titkosítja az adatok előtt tárolásakor toostorage és visszafejtése beolvasása előtt. További információkért lásd: [Azure Storage szolgáltatás titkosítási inaktív adatok](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 inaktív adatok titkosítása ügyfél és kiszolgáló egyaránt támogatja. További információkért lásd: [védelmének használatával adattitkosítás](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Jelenleg adat-előállító nem támogatja a Amazon S3 virtuális magánfelhőbe (VPC) belül.

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift támogatja a fürt titkosítást az inaktív adatok. További információkért lásd: [Amazon Redshift az adatbázis-titkosítás](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Adat-előállító jelenleg nem támogatja a Amazon Redshift egy VPC belül. 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform titkosítási, amely lehetővé teszi az összes olyan fájlok, mellékleteket, egyéni mezők titkosítási támogatja. További információkért lásd: [Web Server OAuth hitelesítési Flow ismertetése hello](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Hibrid környezetekben (az adatkezelési átjáró használatával)
Hibrid forgatókönyvekben szükséges az adatkezelési átjáró toobe telepítve egy helyi hálózaton vagy egy virtuális hálózatot (Azure) vagy virtuális magánfelhőbe (Amazon) belül. hello átjáró képes tooaccess hello helyi adattárolókhoz kell lennie. Hello átjáró kapcsolatos további információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md). 

![Adatkezelési átjáró adatcsatorna](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

Hello **parancscsatornát** adatátviteli szolgáltatások a Data Factory és az adatkezelési átjáró közötti kommunikáció lehetővé teszi. hello kommunikációs ismerteti a kapcsolódó toohello tevékenység. hello adatcsatorna szolgál az adatok átviteléhez a helyszíni adattárolókhoz és felhő adattárolók között.    

### <a name="on-premises-data-store-credentials"></a>A helyszíni adatok adattárolóhoz használandó hitelesítő adatok
a helyszíni adattárolókhoz hello hitelesítő adatai helyben tárolódnak (hello felhőben nem). Három különböző módon állítható. 

- Használatával **egyszerű szöveges** (kevésbé biztonságos) HTTPS Azure-portálon keresztül / másolása varázsló. az átadott hello hitelesítő adatokat egyszerű szöveges toohello helyszíni átjáró vannak.
- Használatával **másolása varázsló JavaScript titkosítás kódtárat**.
- Használatával **kattintson-alapú hitelesítő adatokat kezelő alkalmazások után**. hello kattintson-kérelem végrehajtása a hello a helyi számítógépen, amelynek hozzáférési toohello átjáró, és hello adattár hitelesítő adatok beállítása után. Ezt a beállítást és hello mellett egy biztosan hello legbiztonságosabb beállításokat. hello credential manager alkalmazást, és alapértelmezés szerint hello portot használja 8050 gépen hello átjáró a biztonságos kommunikáció érdekében.  
- Használjon [New-AzureRmDataFactoryEncryptValue](/powershell/module/azurerm.datafactories/New-AzureRmDataFactoryEncryptValue) PowerShell parancsmag tooencrypt hitelesítő adatokat. hello parancsmagot, hogy az átjáró konfigurált toouse tooencrypt hello hitelesítő adatokat az hello tanúsítványt használ. Ez a parancsmag által visszaadott hello titkosított hitelesítő adatokat használja, és adja hozzá túl**EncryptedCredential** hello eleme **connectionString** hello JSON-fájl, amelyet hello [ Új AzureRmDataFactoryLinkedService](/powershell/module/azurerm.datafactories/new-azurermdatafactorylinkedservice) parancsmag vagy a hello JSON részlet a Data Factory Editor hello hello portálon. Ez a beállítás és hello kattintson – Ha az alkalmazás hello legbiztonságosabb beállításokat. 

#### <a name="javascript-cryptography-library-based-encryption"></a>JavaScript titkosítás library-alapú titkosítás
Adatok adattárolóhoz használandó hitelesítő adatok használatával titkosíthatja [JavaScript titkosítás könyvtár](https://www.microsoft.com/download/details.aspx?id=52439) a hello [másolása varázsló](data-factory-copy-wizard.md). Ha ezt a lehetőséget választja, a hello másolása varázsló lekéri az átjáró nyilvános kulcsát hello, és használja tooencrypt hello adatok adattárolóhoz használandó hitelesítő adatok. hello hitelesítő adatokat visszafejteni hello átjárót működtető gépen, és a Windows által védett [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Támogatott böngészők:** IE8, az IE9, IE10, IE11, Microsoft Edge és a legújabb Firefox, Chrome, Opera, Safari böngésző. 

#### <a name="click-once-credentials-manager-app"></a>Kattintson a-egyszer hitelesítő alkalmazással
Indítja el a hello kattintson-után credential manager alkalmazást, az Azure portál vagy másolási varázsló alapú folyamatok runboookok létrehozásakor. Ez az alkalmazás biztosítja, hogy hitelesítő adatok nem kerülnek egyszerű szöveges hello hálózaton keresztül. Alapértelmezés szerint hello portot használja **8050** gépen hello átjáró a biztonságos kommunikáció érdekében. Ha szükséges, a port megváltoztatható.  
  
![Hello átjáró HTTPS-port](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Jelenleg az adatkezelési átjáró használja egyetlen **tanúsítvány**. Ez a tanúsítvány hello gateway telepítés során jön létre (vonatkozik az adatkezelési átjáró 2016. November után létrehozott tooData és verzió 2.4.xxxx.x vagy újabb). Ezt a tanúsítványt lecserélheti a saját SSL/TLS-tanúsítványt. Ezzel a tanúsítvánnyal által hello kattintson-után hitelesítőadat-kezelő alkalmazás toosecurely csatlakozás toohello átjárót működtető gépen adatok adattárolóhoz használandó hitelesítő adatok beállításához. Tárolja az adatokat adattárolóhoz használandó hitelesítő adatok biztonságos helyen a helyi hello Windows segítségével [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) átjáró hello gépen. 

> [!NOTE]
> Régebbi November 2016 előtt vagy a verziójával 2.3.xxxx.x telepített átjárók toouse hitelesítő adatok titkosítása és a felhőben tárolt továbbra is. Akkor is, ha hello átjáró toohello legújabb verzióra frissít, hello hitelesítő adatok nem áttelepített tooan a helyi számítógépen    
  
| Az átjáró verziójának (létrehozásakor) | Tárolt hitelesítő adatok | Hitelesítőadat-titkosítási / biztonsági | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | A felhőben | Titkosított (eltérő hitelesítő adatok manager alkalmazás által használt egyik hello) tanúsítvánnyal | 
| > = 2.4.xxxx.x | A helyszíni | DPAPI védi | 
  

### <a name="encryption-in-transit"></a>Az átvitel során titkosítás
Biztonságos csatornán keresztül van minden adatátviteli **HTTPS** és **TCP-n keresztül TLS** tooprevent-átjárójának támadások az Azure-szolgáltatásokkal folytatott kommunikáció során.
 
Is [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) vagy [Express Route](../expressroute/expressroute-introduction.md) toofurther hello biztonságos kommunikációs csatornát a helyszíni hálózat és az Azure között.

Virtuális hálózat, a hálózat hello felhő logikai reprezentációjává. Egy helyi hálózati tooyour Azure virtuális hálózathoz (VNet) kapcsolódhatnak IPSec VPN-(pont-pont) vagy Express Route (magánhálózati társviszony-létesítés) beállítása       

hello következő táblázat összefoglalja hello hálózati és az átjáró javaslatok alapján különböző kombinációival hibrid adatátvitelt jelölik a forrás és cél helyét.

| Forrás | Cél | Hálózati konfiguráció | Átjáró beállítása |
| ------ | ----------- | --------------------- | ------------- | 
| Helyszíni követelmények | Virtuális gépek és felhőszolgáltatások üzembe helyezett virtuális hálózatok | IPSec VPN (pont – hely vagy pont-pont) | Átjáró lehet telepítve a helyi vagy egy Azure virtuális gép (VM) virtuális hálózat | 
| Helyszíni követelmények | Virtuális gépek és felhőszolgáltatások üzembe helyezett virtuális hálózatok | Az ExpressRoute (magánhálózati társviszony-létesítés) | Átjáró lehet telepítve a helyi vagy egy Azure virtuális gépen a virtuális hálózat | 
| Helyszíni követelmények | Azure-alapú szolgáltatások, amelyek egy nyilvános végpontot | Az ExpressRoute (nyilvános Társviszony) | Átjáró helyszíni telepítve kell lennie. | 

hello következő lemezképek megjelenítése az adatkezelési átjáró hello használata az adatok áthelyezése egy helyi adatbázist és az Azure-szolgáltatások expressroute- és IPSec VPN-(a virtuális hálózattal) használatával:

**Express Route:**
 
![Használjon Expressroute-átjáróval](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN-átjáróval](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Tűzfalbeállítások és átjáró engedélyezett IP-címe

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>A helyi/InPrivate-hálózati tűzfal követelményei  
Vállalati a **vállalati tűzfal** hello szervezet hello központi útválasztón futtatja. Emellett a **Windows tűzfal** démonként fut, a helyi számítógépen hello mely hello átjáró telepítve van. 

hello következő táblázat **kimenő port** és hello tartomány követelményei **vállalati tűzfal**.

| Tartománynevek | Kimenő portok | Leírás |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Hello átjáró tooconnect toodata adatátviteli szolgáltatások adat-előállítóban szükséges |
| `*.core.windows.net` | 443 | Hello használatakor hello átjáró tooconnect tooAzure Tárfiókot használja [másolási előkészített](data-factory-copy-activity-performance.md#staged-copy) szolgáltatás. | 
| `*.frontend.clouddatahub.net` | 443 | Hello átjáró tooconnect toohello Azure Data Factory szolgáltatásnak szüksége. | 
| `*.database.windows.net` | 1433   | (Nem kötelező) szükséges, ha a cél az Azure SQL Database az Azure SQL Data Warehouse- /. Használjon hello másolási szolgáltatás toocopy adatok tooAzure SQL Database vagy az Azure SQL Data Warehouse előkészített hello 1433-as port megnyitása nélkül. | 
| `*.azuredatalakestore.net` | 443 | (Nem kötelező) szükséges, ha a célként megadott Azure Data Lake store | 

> [!NOTE] 
> Előfordulhat, hogy toomanage portok / engedélyezett tartományok: hello vállalati tűzfal szinten szükséges megfelelő adatforrások. Ez a táblázat csak akkor használja az Azure SQL Database, az Azure SQL Data Warehouse, az Azure Data Lake Store példaként.   

hello következő táblázat **port bejövő** hello követelményei **windows tűzfal**.

| Bejövő portok | Leírás | 
| ------------- | ----------- | 
| 8050 (TCP) | Hello credential manager alkalmazás toosecurely a hitelesítő adatokat a helyszíni adattárolókhoz hello átjárón által igényelt. | 

![Átjáró portokra vonatkozó követelmények](media\data-factory-data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP-konfigurációk / engedélyezett az adatok tárolásához
Néhány adattárolókhoz hello felhőben is szükség lehet hozzájuk férni hello gép IP-címet az engedélyezett. Győződjön meg arról, hogy hello átjárót működtető gépen hello IP-címe szerepel az engedélyezési listán / megfelelően konfigurálva a tűzfalon.

hello következő felhőalapú adattároló megkövetelése az engedélyezett IP-címe hello átjárót működtető gépen. Alapértelmezés szerint ezek adattárolókhoz némelyike nem igényel hello IP-címet az engedélyezett. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../documentdb/documentdb-firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Kérdés:** hello átjáró meg lehet osztani összes különböző adat-előállítók?
**Válasz:** még nem támogatott ez a szolgáltatás. Folyamatosan dolgozunk a rajta.

**Kérdés:** Mik a hello átjáró toowork hello portokra vonatkozó követelmények?
**Válasz:** átjáró hoz meg HTTP-alapú kapcsolatok tooopen internet. Hello **443-as és a 80-as kimenő portok** meg kell nyitni az átjáró toomake ezt a kapcsolatot. Nyissa meg **bejövő Port 8050** csak szintjén hello gép (nem a vállalati tűzfal szintjén) hitelesítőadat-kezelő alkalmazáshoz. Azure SQL Database vagy az Azure SQL Data Warehouse használata regisztrációja, mivel a forrás / cél, akkor kell tooopen **1433** portot is. További információkért lásd: [tűzfal-konfiguráció, és az engedélyezett IP-címek](#firewall-configurations-and-whitelisting-ip-address-of gateway) szakasz. 

**Kérdés:** Mik átjáró tanúsítványokkal kapcsolatos követelményei?
**Válasz:** aktuális átjáró biztonságosan az adatokat tároló hitelesítő adatok beállítására hello credential manager alkalmazás által használt tanúsítványt igényel. Ez a tanúsítvány önaláírt tanúsítvány létrehozása és konfigurálása hello átjáró telepítő. Használhatja a saját TLS / SSL-tanúsítványt helyette. További információkért lásd: [kattintson-egyszer hitelesítőadat-kezelő alkalmazás](#click-once-credentials-manager-app) szakasz. 

## <a name="next-steps"></a>Következő lépések
A másolási tevékenység teljesítményével kapcsolatos információkért lásd: [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).

 
