---
title: "Elemzés platformok: Apache Storm összehasonlító tooStream Analytics |} Microsoft Docs"
description: "Útmutató a felhőalapú analytics platform kiválasztása az Apache Storm összehasonlító tooStream Analytics segítségével beolvasása. Szolgáltatások és különbségek megértése."
keywords: "elemzési platformot, analytics platform, elemzés felhőplatform, storm összehasonlítása"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: 5a0ec5b2439596f0da962f04b776472031660062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a>A folyamatos átviteli analytics platform kiválasztása: az Apache Storm és az Azure Stream Analytics összehasonlítása
Azure adatfolyam-továbbítási adatok elemzése több megoldást nyújt: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) és [Azure HDInsight alatt futó Apache Storm](https://azure.microsoft.com/services/hdinsight/apache-storm/). Mindkét analytics platform előnyei hello a PaaS megoldás. De hello platformok jelentős eltérések képességeit, valamint hogyan konfigurálhatja és kezelheti azokat. 

Ez a cikk választhat alatt futó Apache Storm és Azure Stream Analytics felhő analytics platformként között szolgáltatások toohelp egymás melletti összehasonlítását tartalmazza. 

## <a name="general-features"></a>Általános szolgáltatások

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Az Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Nyílt forráskódú?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nem. Az Azure Stream Analytics a Microsoft védett kínál.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Igen. Apache Storm egy olyan licenccel rendelkező Apache technológia.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>A Microsoft támogatási?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Igen </p>
            </td>
            <td width="246" valign="top">
                <p>
Igen </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardverkövetelmények</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
nincs. Az Azure Stream Analytics egy olyan Azure-szolgáltatás.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
nincs. Apache Storm egy olyan Azure-szolgáltatás.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Legfelső szintű egység</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Felhasználók telepítheti és figyelheti a folyamatos átviteli feladat.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználók központi telepítése, és figyelje az egész fürt, amelyek több Storm feladatok, valamint egyéb munkaterhelések (beleértve a kötegelt) tartalmazhatnak.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Tarifacsomag</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Által feldolgozott adatmennyiség áron és hello adatfolyam-továbbítási egységek száma óránként szükséges hello feladat fut-e. 
                </p>
                    <p>További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Díjszabásáról</a>.</p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
hello beszerzési fürt-alapú, és hello fürt fut, független telepített feladatok hello idővel feladata alapján.
                </p>
                <p>
További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight árképzési</a>.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a>Szerzői

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Az Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Képességek: SQL DSL?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Igen. A Stream Analytics egy SQL-szerű nyelv nyújt átalakítások létrehozásához.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nem. Felhasználók írhat kódot a Java vagy C# vagy Trident az API-kkal.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Képességek: Az ideiglenes operátorok?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ablakos összesítéseket és az időalapú illesztéseket alapértelmezés szerint támogatott.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Az ideiglenes operátorok hello felhasználónak kell végrehajtania.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Fejlesztési felület</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Felhasználók létrehozása, hibakeresési, és figyelje a feladatokat a hello Azure-portálon keresztül mintaadatok származó élő adatfolyam használata.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználók a .NET fejlesztés, hibakeresését és figyelése a Visual Studio alkalmazással. Java vagy egyéb nyelvek felhasználók használhatják az általuk választott IDE hello.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hibaelhárítási támogatás</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Alapszintű állapotának és műveleteinek feladatnaplóit elérhető toohelp hibakeresési. A Stream Analytics jelenleg megadásakor a felhasználók nem adja meg a tartalmat, vagy mekkora a tartalom megtalálható hello naplók (azaz részletes üzemmód).
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Részletes naplózási érhetők el. A felhasználók naplók a Visual Studio vagy toohello fürt naplózás és hello naplók elérése közvetlenül.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>A bővítési egyéni kód használatával?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Részlegesen támogatja a JavaScript felhasználó által megadott függvények rendelkező. További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integrációs</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Igen. Felhasználók is egyéni kód írását, a C#, Java, vagy bármely más, a Storm támogatott nyelven.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a>Adatforrások (bemeneti) és -kimenetek ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Az Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Bemeneti adatforrások</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Az Azure Event Hubs és az Azure Blob storage.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Összekötők Azure Event Hubs, Azure Service Bus, Kafka és több érhetők el. Felhasználók hozhatnak létre további összekötők egyéni kód használatával.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>A bemeneti adatok formátumok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Az Avro, JSON, CSV </p>
            </td>
            <td width="246" valign="top">
                <p>
Tetszőleges méretű egyéni kód használatával a felhasználók is végrehajthatják.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kimenetek</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
A folyamatos átviteli feladatnak rendelkezhet több kimenet. Támogatott kimenetek az Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL Database és Power bi-ban.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm sok kimenetek támogatja a topológiában, és minden egyes kimeneti lehet alárendelt feldolgozási egyéni logikát. A Storm összekötők Power bi-ban, az Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL és HBase magában foglalja. Felhasználók hozhatnak létre további összekötők egyéni kód használatával.    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Adatok kódolási formátum</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Adatok UTF-8 használatával kell formázni.
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
Felhasználók bármilyen egyéni kód használatával adatok kódolási formátum is létrehozható.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a>Felügyelete és műveletei ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Az Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Feladat telepítési modell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure-portálon, a PowerShell és a REST API-k.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Azure-portálon, a PowerShell, a Visual Studio és a REST API-k.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Monitoring (statisztikák, számlálók, stb.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Figyelése Azure portál és a REST API-k segítségével történik. Felhasználók is konfigurálhatja az Azure riasztásokat.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Figyelési hello Storm felhasználói felülete és a REST API-k segítségével történik.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Méretezhetőség</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Méretezhetőség hello minden folyamatos átviteli egységek (SUs) számát határozza meg. Minden Streaming Unit dolgozza fel a too1 mentése MB/másodperc, a maximális 50 egységek használatával. További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">méretezési tooincrease átviteli</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Méretezhetőség hello hello HDInsight alatt futó Storm-fürt a csomópontok száma határozza meg. hello felső korlátot hello csomópontok hello felhasználói Azure kvóta határozzák meg.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Adatok feldolgozási korlátok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Felhasználók növelheti az adatok feldolgozása vagy optimalizálni a költségeket növelésével vagy csökkentésével hello és a felső korlátja 1 GB/s Streaming Units számát.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználók méretezheti fürtméret felfelé vagy lefelé.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Állítsa le vagy folytatása</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Állítsa le, és indítsa újra az utolsó helyen leállt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Legutóbbi folytatni helyezze leállított vízjel alapján.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Frissítési szolgáltatás és -keretrendszer</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatikus javítás állásidő nélkül.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatikus javítás állásidő nélkül.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Az üzletmenet folytonossága garantált SLA-k a magas rendelkezésre álló szolgáltatáson keresztül</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li>Szolgáltatásiszint-szerződésben garantált 99,9 %-os hasznos üzemidő</li>
                <li>A hibák automatikus helyreállítás</li>
                <li>Állapot-nyilvántartó az ideiglenes operátorok beépített helyreállítása</li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
Szolgáltatásiszint-szerződésben garantált 99,9 %-os üzemidőt hello Storm-fürt. 
                </p>
                <p>
Apache Storm egy hibatűrő adatfolyam platform. Azonban, hogy a folyamatos átviteli feladatok folyamatos Futtatás hello felhasználó felelőssége tooensure.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a>Speciális funkciók ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Az Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Késő érkezés és soron eseménykezelésnek</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Beépített konfigurálható házirendek események sorrendjének módosításához, dobja el az események, vagy állítsa be az esemény időpontja.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Felhasználók meg kell valósítania logika toohandle ebben a forgatókönyvben.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Referenciaadatok</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Az Azure Blob storage egy legfeljebb 100 MB memória gyorsítótárának referenciaadatok érhető el. Referenciaadatok hello szolgáltatás által frissül.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nem határoz meg az adatok mérete. Összekötők HBase, az Azure Cosmos adatbázis, az SQL Server és az Azure érhetők el. Felhasználók hozhatnak létre további összekötők egyéni kód használatával. Referenciaadatok frissíteni kell az egyéni kód használatával.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integráció a gépi tanulás</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Közzétett Azure Machine Learning modellek konfigurálhatók funkciók feladat létrehozása során. További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">skálája a Machine Learning funkciók</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
A Storm Boltokhoz keresztül elérhető.
                </p>
            </td>
        </tr>
    </tbody>
</table>
