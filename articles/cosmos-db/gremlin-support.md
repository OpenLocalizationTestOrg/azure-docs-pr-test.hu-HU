---
title: "aaaAzure Cosmos DB Gremlin támogatási |} Microsoft Docs"
description: "További információk az Apache TinkerPop, amely szolgáltatásairól, és lépések Gremlin nyelvi hello és elérhető az Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 6016ccba-0fb9-4218-892e-8f32a1bcc590
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/10/2017
ms.author: denlee
ms.openlocfilehash: f8c2ce50c6946e971f56fe1f3838b0899cb2ad8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Azure Cosmos DB Gremlin graph-támogatás
Azure Cosmos-adatbázis támogatja [Apache Tinkerpop](http://tinkerpop.apache.org) átjárás nyelvi diagramot [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), vagyis egy grafikonon API graph entitások létrehozására és a graph lekérdezés műveletet hajt végre. Hello Gremlin nyelvi toocreate graph entitások (csúcsban és szélek) használja, belül entitásokból tulajdonságainak módosítása, hajtsa végre a lekérdezéseket és traversals és entitások törlésére. 

Az Azure Cosmos DB számos lehetőséget kínál vállalati használatra kész szolgáltatások toograph adatbázisok. Ez magában foglalja a globális terjesztési, független méretezhetővé tárolási és átviteli, előre jelezhető egyjegyű ezredmásodperces késések fordulnak elő, az automatikus indexeléshez és 99,99 % SLA-k száma. Mivel az Azure Cosmos DB TinkerPop/Gremlin támogatja, egyszerűen áttelepítheti egy másik graph-adatbázis segítségével anélkül, hogy toomake kódmódosításokat írt alkalmazások. Emellett Gremlin támogatási címtár Azure Cosmos DB zökkenőmentesen integrálható a TinkerPop-kompatibilis analytics keretrendszerek, például a [Apache Spark GraphX](http://spark.apache.org/graphx/). 

Ez a cikk azt ismertetik a gyors Gremlin, és hello Gremlin szolgáltatások és a Graph API-támogatást előnézete hello támogatott lépéseket számbavétele.

## <a name="gremlin-by-example"></a>Példa alapján gremlin
Most használja egy minta graph toounderstand hogyan lekérdezések Gremlin lehet megadni. hello alábbi ábrán egy üzleti alkalmazás, amely felügyeli a felhasználók, érdeklődési és egy grafikonon hello formájában eszközök adatait.  

![Személyek, eszközök és érdeklődési mintaadatbázis](./media/gremlin-support/sample-graph.png) 

Ehhez a diagramhoz tartozik hello csúcspont típusok ("címke" néven Gremlin) a következő:

- Személyek: hello diagramnak van három személyek, multiplexelés Thomas és Ben
- Érdeklődési: Ebben a példában az érdeklődési hello bemutatjuk játék
- Eszközök: személyek használó hello eszközök
- Operációs rendszerek esetén: hello eszközökön futó hello operációs rendszerek

Azt mutatják be ezeket az entitásokat a következő biztonsági típusok és címkéket hello keresztül hello kapcsolatai:

- Tudja: például "Thomas tudja multiplexelés"
- Érdekelt: a Graph hello személyek, például "Ben olyan iránt érdeklődik bemutatjuk" toorepresent hello érdekében
- RunsOS: A hordozható számítógép hello Windows operációs rendszer fut
- Felhasználás: toorepresent egy személy eszköz használja. Például multiplexelés használ sorozatszámú 77 Motorola telefon

Egyes műveletek most futtatni ehhez a diagramhoz hello segítségével [Gremlin konzol](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console). Ezeket a műveleteket Gremlin illesztőprogramok hello platform az Ön által választott (Java, Node.js, Python vagy .NET) használatával is elvégezheti.  Mielőtt úgy tekintünk, mi az Azure Cosmos Adatbázisba támogatott vizsgáljuk meg néhány példa tooget ismeri a hello szintaxisát.

Első CRUD vizsgáljuk meg. hello következő Gremlin utasítás illeszt hello "Thomas" csúcspont hello graph:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

A következő Gremlin utasítás a következő hello szúr be egy "tudja" peremhálózati Thomas és multiplexelés között.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

hello következő lekérdezés adja vissza hello "személy" csúcsban csökkenő sorrendben első neveik:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Ha diagramokat izzólámpákhoz akkor, ha szüksége tooanswer kérdések például a "milyen operációs rendszerek Thomas barátok használnak?". Futtathatja a egyszerű Gremlin átjárás tooget ezt az információt hello grafikonból:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Most nézzük Azure Cosmos DB tartalma Gremlin fejlesztők számára.

## <a name="gremlin-features"></a>Gremlin szolgáltatások
TinkerPop olyan szabvány, amely számos különböző graph technológiák lefedi. Ezért van ismertetésében toodescribe milyen funkciók egy grafikonon szolgáltató által biztosított. Azure Cosmos-adatbázis egy állandó, nagy feldolgozási, írható graph-adatbázis, amely több kiszolgálók vagy fürt lehet particionálni biztosít. 

hello következő táblázat sorolja fel, amelyeket a rendszer Azure Cosmos DB hello TinkerPop szolgáltatások: 

| Kategória | Az Azure Cosmos DB végrehajtása |  Megjegyzések | 
| --- | --- | --- |
| Graph-funkciók | Előzetes adatmegőrzési és ConcurrentAccess nyújt. Tervezett toosupport tranzakciók | Számítógép módszerek hello Spark-összekötőn keresztül valósítható meg. |
| Változó szolgáltatások | Támogatja a logikai, egész, bájt, duplán, lebegőpontos, egész, hosszú, karakterlánc | Támogatja az egyszerű típusok, kompatibilis adatmodellt összetett típus |
| Csúcspont szolgáltatások | Támogatja a RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty  | Támogatja a létrehozása, módosítása és törlése csúcsban |
| Csúcspont tulajdonság szolgáltatások | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Támogatja a létrehozása, módosítása és törlése csúcspont tulajdonságai |
| Biztonsági szolgáltatások | AddEges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Támogatja a létrehozása, módosítása és törlése élei számára |
| Edge tulajdonság szolgáltatások | Tulajdonságok, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Támogatja a létrehozása, módosítása és törlése peremhálózati tulajdonságai |

## <a name="gremlin-wire-format-graphson"></a>Gremlin egybeírt: GraphSON

Az Azure Cosmos DB használ hello [GraphSON formátum](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) Gremlin származó eredmények visszaadásakor. GraphSON hello Gremlin szabványos formátuma csúcsban, szélek és tulajdonságok (egy- és többértékű tulajdonságai) használatával JSON. 

Például hello alábbi kódrészletben láthatja a csúcspont GraphSON megjelenítése *toohello ügyfél visszaadott* a Azure Cosmos-Adatbázisból. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

a rendszer hello következő csúcsban GraphSON által használt hello tulajdonságokat:

| Tulajdonság | Leírás |
| --- | --- |
| id | hello csúcspont hello azonosítója. (A kombinációja hello értékű _partition, ha van ilyen) egyedinek kell lennie |
| Címke | hello csúcspont hello címkéje. Ez az opcionális és használt toodescribe hello entity Type típusként. |
| type | Használt toodistinguish csúcsban nem graph dokumentumból |
| properties | Felhasználói tulajdonságok hello csúcspont társított összessége. Minden egyes tulajdonsága több értékeket veheti fel. |
| _partition (konfigurálható) | hello csúcspont hello partíciókulcs. Toomultiple kiszolgálók lehetnek kimenő diagramjait használt tooscale |
| outE | Ez a csúcspont szélek kimenő listáját tartalmazza. Csúcspont hello Simuló adatok tárolása lehetővé teszi a traversals gyors végrehajtását. Szegély a címkék alapján vannak csoportosítva. |

És hello peremhálózati tartalmazza a következő információk toohelp navigációs tooother alkotórészek hello graph hello.

| Tulajdonság | Leírás |
| --- | --- |
| id | hello peremhálózati hello azonosítója. (A kombinációja hello értékű _partition, ha van ilyen) egyedinek kell lennie |
| Címke | hello peremhálózati hello címkéje. Ez a tulajdonság nem kötelező, és a használt toodescribe hello kapcsolattípus. |
| Leltárjának | Az él csúcsban lévő listáját tartalmazza. Hello Simuló információk tárolására hello oldala lehetővé teszi a traversals gyors végrehajtását. Csúcsban a címkék alapján vannak csoportosítva. |
| properties | Felhasználói tulajdonságok hello peremhálózati társított összessége. Minden egyes tulajdonsága több értékeket veheti fel. |

Minden egyes tulajdonság szerepel egy tömbben több érték is tárolható. 

| Tulajdonság | Leírás |
| --- | --- |
| érték | hello hello tulajdonság értéke

## <a name="gremlin-partitioning"></a>Particionálás gremlin

Az Azure Cosmos DB, diagramjait belül is méretezhető tárolókhoz tároló egymástól függetlenül a tárolási és átviteli sebesség (a normalizált kérelmek / másodperc szerint megadva) tekintetében. Minden egyes tárolóban kell adnia egy nem kötelező, de ajánlott a partíciós kulcs tulajdonság határozza meg, hogy a logikai partíciót határ kapcsolódó adatok. Minden csomópont él kell lennie egy `id` tulajdonság, amely egyedi entitások belül, hogy a partíciós kulcs értéke. hello részleteket ismertetnek [Azure Cosmos DB a particionálás](partition-data.md).

Gremlin műveletek problémamentesen működik, amelyek több partíciót az Azure Cosmos DB több Diagramadatok között. Azonban ajánlott a partíciós kulcs toochoose a diagramok szűrőként lekérdezéseket, a gyakran használt sok különböző értéket tartalmaz, és hasonló gyakorisága elérni ezeket az értékeket. 

## <a name="gremlin-steps"></a>Gremlin lépéseket
Most nézzük hello Azure Cosmos DB által támogatott Gremlin lépéseket. A Gremlin teljes referenciáért lásd: [TinkerPop hivatkozás](http://tinkerpop.apache.org/docs/current/reference).

| Lépés | Leírás | TinkerPop 3.2 dokumentáció | Megjegyzések |
| --- | --- | --- | --- |
| `addE` | Két csúcsban közötti él hozzáadása | [addE lépés](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) | |
| `addV` | Hozzáadja a csúcspont toohello diagramhoz | [addV lépés](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) | |
| `and` | Ensurest, hogy az összes hello traversals ad vissza értéket | [lépés](http://tinkerpop.apache.org/docs/current/reference/#and-step) | |
| `as` | A lépés modulátor tooassign egy változó toohello kimenet egy lépés | [lépéseként](http://tinkerpop.apache.org/docs/current/reference/#as-step) | |
| `by` | A lépés modulátor használt `group` és`order` | [lépés](http://tinkerpop.apache.org/docs/current/reference/#by-step) | |
| `coalesce` | Hello első átjárás, amely visszaadja az eredményt adja vissza | [a Coalesce lépés](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) | |
| `constant` | Konstans értéket ad vissza. Együtt`coalesce`| [állandó lépés](http://tinkerpop.apache.org/docs/current/reference/#constant-step) | |
| `count` | Hello átjárás hello számát adja vissza | [Count lépés](http://tinkerpop.apache.org/docs/current/reference/#count-step) | |
| `dedup` | Értéket ad vissza értéket hello az hello ismétlődő értékek eltávolításával | [a deduplikáció lépés](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) | |
| `drop` | Elhagyta hello értékek (csúcspont/oldal) | [közvetlen lépés](http://tinkerpop.apache.org/docs/current/reference/#drop-step) | |
| `fold` | A korlát, amely kiszámítja az eredmények hello összesítés sújtó| [modellrészek lépés](http://tinkerpop.apache.org/docs/current/reference/#fold-step) | |
| `group` | Csoportok hello megadott hello címkék alapján érték| [a csoport lépés](http://tinkerpop.apache.org/docs/current/reference/#group-step) | |
| `has` | Használt toofilter tulajdonságait, csúcsban és szélén. Támogatja a `hasLabel`, `hasId`, `hasNot`, és `has` Variant adattípusban. | [lépés rendelkezik](http://tinkerpop.apache.org/docs/current/reference/#has-step) | |
| `inject` | Értékek behelyezése adatfolyam| [lépés beszúrása](http://tinkerpop.apache.org/docs/current/reference/#inject-step) | |
| `is` | Használt tooperform egy szűrő segítségével egy logikai kifejezés | [lépés](http://tinkerpop.apache.org/docs/current/reference/#is-step) | |
| `limit` | Hello átjárás használt toolimit elemek száma| [korlát lépés](http://tinkerpop.apache.org/docs/current/reference/#limit-step) | |
| `local` | Helyi becsomagolja könyvtármegkerülési, hasonló tooa segédlekérdezés szakasz | [helyi lépés](http://tinkerpop.apache.org/docs/current/reference/#local-step) | |
| `not` | A szűrt tooproduce hello tagadásának használt | [nem lépés](http://tinkerpop.apache.org/docs/current/reference/#not-step) | |
| `optional` | Értéket ad vissza a megadott hello eredményét hello átjárás azt eredményez, más eredményt adja vissza, ha a hívó elem hello | [nem kötelező lépés](http://tinkerpop.apache.org/docs/current/reference/#optional-step) | |
| `or` | Biztosítja a hello traversals közül legalább egy értéket ad vissza | [vagy lépés](http://tinkerpop.apache.org/docs/current/reference/#or-step) | |
| `order` | A megadott rendezési sorrend hello eredményeket ad vissza | [rendelés lépés](http://tinkerpop.apache.org/docs/current/reference/#order-step) | |
| `path` | Beolvasása hello hello átjárás elérési útját | [elérési út lépés](http://tinkerpop.apache.org/docs/current/reference/#path-step) | |
| `project` | Mivel a projektek hello tulajdonságai | [Projekt lépés](http://tinkerpop.apache.org/docs/current/reference/#project-step) | |
| `properties` | A megadott címkék hello hello tulajdonságainak beolvasása | [Tulajdonságok lépés](http://tinkerpop.apache.org/docs/current/reference/#properties-step) | |
| `range` | Szűrők toohello megadott értékek| [tartomány lépés](http://tinkerpop.apache.org/docs/current/reference/#range-step) | |
| `repeat` | Ismétlődik hello megoldás a hello megadott számú alkalommal. Ismétlési használt | [Ismételje meg a](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) | |
| `sample` | Használt hello átjárás toosample eredményei | [a minta lépés](http://tinkerpop.apache.org/docs/current/reference/#sample-step) | |
| `select` | Használt hello átjárás tooproject eredményei |  [Válassza ki a lépés](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Összesítések nem blokkoló a hello átjárás használatos | [tároló lépés](http://tinkerpop.apache.org/docs/current/reference/#store-step) | |
| `tree` | Egy fába csúcspont összesített görbék | [fa lépés](http://tinkerpop.apache.org/docs/current/reference/#tree-step) | |
| `unfold` | Az iterátor unroll lépésként| [unfold lépés](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) | |
| `union` | Több traversals eredményeinek egyesítése| [a UNION lépés](http://tinkerpop.apache.org/docs/current/reference/#union-step) | |
| `V` | Lépésekből hello traversals szükséges között és a csúcsban `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV`, és `otherV` számára | [csúcspont lépései](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) | |
| `where` | Hello átjárás toofilter eredményeinek használt. Támogatja a `eq`, `neq`, `lt`, `lte`, `gt`, `gte`, és `between` operátorok  | [Ha a lépés](http://tinkerpop.apache.org/docs/current/reference/#where-step) | |

Azure Cosmos adatbázis-írási optimalizált kezelő támogatja az automatikus indexeléshez levő összes tulajdonság belül és a csúcsban alapértelmezés szerint. Ezért lekérdezi tartománnyal lekérdezések rendezésére, szűrők, vagy bármely tulajdonság összesítések hello index a feldolgozott és hatékony és kiszolgálása között. További információ a hogyan indexelési működéséről az Azure Cosmos Adatbázisba, lásd a dokumentum [séma-független indexelő](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Következő lépések
* Első lépések egy grafikonon alkalmazás felépítése [az SDK-k használatával](create-graph-dotnet.md) 
* További információ [Azure Cosmos DB graph-támogatás](graph-introduction.md)
