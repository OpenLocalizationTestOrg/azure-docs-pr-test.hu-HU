---
title: "az SQL Server biztonsági mentése és visszaállítása az Azure storage aaaHow toouse |} Microsoft Docs"
description: "Megtudhatja, hogyan tooback SQL Server tooAzure tárolási fel. Biztonsági mentése SQL-adatbázisok tooAzure tárolási hello előnyeit ismerteti."
services: virtual-machines-windows
documentationcenter: 
author: MikeRayMSFT
manager: jhubbard
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: 67ebe8377be97df1312f8c1345e23576aabe0c4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Az Azure Storage használata az SQL Server biztonsági mentése és visszaállítása
## <a name="overview"></a>Áttekintés
SQL Server 2012 SP1 CU2 verziótól kezdődően írhat SQL Server biztonsági mentés közvetlenül toohello Azure Blob storage szolgáltatásban. A funkció tooback tooand visszaállítási hello Azure Blob szolgáltatás be egy helyi SQL Server-adatbázist vagy egy SQL Server-adatbázis egy Azure virtuális gépen használható. Biztonsági mentési toocloud előnyökkel rendelkezésre állását, korlátlan georeplikált telephelytől távoli tárhely és adatok tooand áttelepítésének megkönnyítése érdekében a hello felhőből. Transact-SQL vagy SMO biztonsági MENTÉSI vagy VISSZAÁLLÍTÁSI utasításokat adhatnak ki.

SQL Server 2016 vezet be az új képességek; használhat [fájlpillanatkép biztonsági mentés](http://msdn.microsoft.com/library/mt169363.aspx) tooperform meglepően gyors visszaállítások és szinte azonnali biztonsági mentés.

Ez a témakör azt ismerteti, miért érdemes választania toouse SQL biztonsági mentés az Azure storage és hello összetevők vesz majd ismerteti. Hello cikk tooaccess forgatókönyvek és további információkat toostart használhatja a szolgáltatást az SQL Server biztonsági másolat hello végén hello erőforrásokat használhatja.

## <a name="benefits-of-using-hello-azure-blob-service-for-sql-server-backups"></a>Using hello Azure Blob szolgáltatás az SQL Server biztonsági mentések előnyei
Több, amikor az SQL Server biztonsági mentéséről szembesülhetnek akadályok merülnek fel. Ezek a kihívások magában foglalhatja tárolási felügyeleti, tárolási meghibásodás kockázatát, toooff-hely tárolási és hardverkonfigurációját. Ezek a kihívások számos szolgáltatással hello Azure Blob tároló SQL Server biztonsági mentés tárgyalja. Vegye figyelembe a következő előnyöket hello:

* **Könnyű használat**: a biztonsági mentések tárolására az Azure-blobokat lehet egy kényelmes, rugalmasan és egyszerűen tooaccess telephelytől távoli lehetőséget. Távoli tároló létrehozása, lehet, hogy az SQL Server biztonsági mentések lehető legkönnyebben módosítja a meglévő parancsfájlok és feladatok toouse hello **biztonsági MENTÉS tooURL** szintaxist. Távoli tároló általában kell távolságra hello éles adatbázis helye tooprevent egyetlen katasztrófa, amelyek hatással lehetnek a telephelytől távol hello és a termelési adatbázisfájlok helyét. Válassza ki a túl[földrajzi-replikálás az Azure-blobok](../../../storage/common/storage-redundancy.md), egy további védelmi réteget hello eseményeket, ha egy olyan vészhelyzet esetén, amely befolyásolhatja hello teljes régióban van.
* **Biztonsági mentési archív**: hello Azure Blob Storage szolgáltatást kínál jobb alternatív toohello gyakran használt szalag beállítás tooarchive biztonsági mentéseket. A szalagos tárolás fizikai szállítására tooan külső létesítmény és intézkedések tooprotect hello adathordozó lehet szükség. A biztonsági mentések tárolására az Azure Blob Storage biztosít egy pillanat alatt elérheti, magas rendelkezésre állású, és a tartós archiválás lehetőséget.
* **Hardver felügyelt**: nincs nem növeli a hardver-kezelés az Azure-szolgáltatásokhoz. Azure-szolgáltatások kezelése hello hardver, és adja meg a georeplikáció redundancia és a hardver meghibásodása elleni védelem.
* **Korlátlan tárterület**: egy közvetlen biztonsági mentési tooAzure blobok engedélyezésével rendelkezik hozzáférési toovirtually korlátlan tárterület. Azt is megteheti biztonsági mentése tooan Azure virtuálisgép-lemez van korlátok alapján a gép méretét. Van korlátja toohello szám lemezek csatolhat a virtuális gép az Azure biztonsági mentések tooan. Ezt a határt egy extra nagy példány 16 lemezek és kisebb példányok kevesebb.
* **Biztonsági mentés a rendelkezésre állási**: Azure-blobokat tárolt biztonsági bárhonnan és bármikor érhetők el, és könnyen lehet érhető el a(z) visszaállítások tooeither egy helyi SQL Server vagy egy Azure virtuális gépet futtat egy másik SQL Server nélkül hello kell az adatbázis-csatlakoztatási/leválasztási vagy letöltése és csatolására hello VHD-t.
* **Költség**: csak a használt hello szolgáltatás után kell fizetnie. Költséghatékony helyszínen és a biztonsági mentés archív lehetőségként lehet. Lásd: hello [Azure árképzési Számológép](http://go.microsoft.com/fwlink/?LinkId=277060 "Díjkalkulátor"), és hello [Azure díjszabása cikk](http://go.microsoft.com/fwlink/?LinkId=277059 "árazás cikk") további információ.
* **Storage-pillanatfelvételekkel**: Ha adatbázisfájlokat tárolja az Azure blob, és az SQL Server 2016 használ, [fájlpillanatkép biztonsági mentés](http://msdn.microsoft.com/library/mt169363.aspx) tooperform meglepően gyors visszaállítások és szinte azonnali biztonsági mentés.

További részletekért lásd: [SQL Server biztonsági mentése és visszaállítása az Azure Blob Storage Service](http://go.microsoft.com/fwlink/?LinkId=271617).

a következő két szakasz hello hello Azure Blob storage szolgáltatás, így az SQL Server-összetevőt szükséges hello vezethet. Fontos fontos toounderstand hello összetevők, illetve a interakció toosuccessfully használjon biztonsági mentési és visszaállítási hello Azure Blob storage szolgáltatásból.

## <a name="azure-blob-storage-service-components"></a>Az Azure Blob Storage szolgáltatás-összetevők
hello Azure következő összetevők használata, amikor biztonsági mentése toohello Azure Blob storage szolgáltatásban.

| Összetevő | Leírás |
| --- | --- |
| **Tárfiók** |hello tárfiók összes tárolószolgáltatások kiindulópont hello kerül. tooaccess egy Azure Blob Storage szolgáltatással, először létre kell hoznia egy Azure Storage-fiókot. Azure Blob storage szolgáltatással kapcsolatos további információkért lásd: [hogyan toouse hello Azure Blob Storage szolgáltatás](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Tároló** |Egy tároló blobok blobkészletek csoportosítását, és korlátlan számú BLOB tárolhatja. egy SQL Server biztonsági másolat tooan Azure Blob szolgáltatás toowrite, rendelkeznie kell legalább hello legfelső szintű tároló létrehozása. |
| **A BLOB** |A fájl típusát és méretét. Blobok olyan megcímezhető hello URL-cím formátuma a következő használatával: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Lapblobokat kapcsolatos további információkért lásd: [Understanding Block és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-összetevők
hello következő SQL Server-összetevőt használja a biztonsági mentése toohello Azure Blob storage szolgáltatásban.

| Összetevő | Leírás |
| --- | --- |
| **URL-CÍME** |Egy URL-címet adja meg az egységes erőforrás-azonosító (URI) tooa egyedi biztonsági másolatból. hello URL-címe: a használt tooprovide hello helyét és hello SQL Server biztonságimásolat-fájl nevét. hello URL-címet kell mutatnia tooan tényleges blob, nem csak egy tárolót. Ha hello blob nem létezik, akkor jön létre. Ha egy meglévő blob van megadva, biztonsági MENTÉS sikertelen lesz, kivéve, ha hello > WITH FORMAT kapcsolót. hello az alábbiakban látható példa lehet megadni a BACKUP parancs hello hello URL-CÍMRE: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS ajánlott, de nem kötelező. |
| **Hitelesítő adatok** |hello tooconnect szükséges információkat, és a hitelesítéshez tooAzure Blob storage szolgáltatás hitelesítő adatot tárol.  Ahhoz, hogy az SQL Server toowrite biztonsági mentések tooan Azure Blob vagy visszaállítási belőle létre kell hozni egy SQL Server hitelesítő adatot. További információkért lásd: [SQL-kiszolgáló hitelesítési](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Ha toocopy választja, és töltse fel a biztonságimásolat-fájl toohello Azure Blob storage szolgáltatásban, kell használnia blob laptípus tárolási lehetőségként Ha azt tervezi, toouse visszaállítási műveletek ezt a fájlt. Egy blob blokktípus való VISSZAÁLLÍTÁSA egy hibával meghiúsul.
> 
> 

## <a name="next-steps"></a>Következő lépések
1. Az Azure-fiók létrehozása, ha még nem rendelkezik. Ha az Azure értékelése, vegye figyelembe a hello [ingyenes próbaverzió](https://azure.microsoft.com/free/).
2. Folytassa a következő oktatóprogramot kínál, amelyek végigvezetik a tárfiók létrehozásához, és a visszaállítási folyamat végrehajtása hello egyikével.
   
   * **Az SQL Server 2014**: [oktatóanyag: SQL Server 2014 biztonsági mentési és visszaállítási tooMicrosoft Azure Blob Storage szolgáltatás](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [oktatóanyag: SQL Server 2016 adatbázisok hello Microsoft Azure Blob storage szolgáltatás használatával](https://msdn.microsoft.com/library/dn466438.aspx)
3. Tekintse át a dokumentációt kezdve [SQL Server biztonsági másolat és helyreállítás a Microsoft Azure Blob Storage szolgáltatás](https://msdn.microsoft.com/library/jj919148.aspx).

Ha bármilyen problémába ütközik, tekintse át a hello témakört [SQL Server biztonsági másolat tooURL ajánlott eljárások és hibaelhárítás](https://msdn.microsoft.com/library/jj919149.aspx).

Más SQL Server biztonsági mentési és visszaállítási eljárások alkalmazhatók, lásd: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).

