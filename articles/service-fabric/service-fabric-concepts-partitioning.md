---
title: "a Service Fabric-szolgáltatások aaaPartitioning |} Microsoft Docs"
description: "Ismerteti, hogyan toopartition Service Fabric állapotalapú szolgáltatások. Partíciók lehetővé teszi, hogy adattárolás hello helyi gépen, adatokat és a számítás is méretezhető együtt."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a>A Service Fabric megbízható szolgáltatások partíció
A cikkben egy bevezető toohello Azure Service Fabric megbízható szolgáltatások particionálás alapvető fogalmait. hello hello cikkben használt forráskód is rendelkezésre áll a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Particionálás
Particionálás nincs egyedi tooService háló. Ez valójában egy alapvető szerkezet méretezhető szolgáltatások. Egy tágabb értelemben véve a azt gondolja át particionálás egy fogalom felosztásával állapota (adatok), és kisebb elérhető egységek tooimprove méretezhetőséget és teljesítményt nyújt a számítási. Egy jól ismert particionálás formátuma [adatparticionálás][wikipartition], más néven horizontális.

### <a name="partition-service-fabric-stateless-services"></a>A Service Fabric állapotmentes szolgáltatások partíció
Állapotmentes szolgáltatások esetén azt is gondolja át éppen egy logikai egységet a szolgáltatás egy vagy több példányát tartalmazó partíció. 1. ábra mutatja egy állapotmentes szolgáltatások elosztva a fürt egy partíciót öt osztályt.

![Állapotmentes szolgáltatások](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Valóban két típusa van állapotmentes szolgáltatások megoldások. hello először egyik olyan szolgáltatás, amely továbbra is fennáll állapotában kívülről, például egy Azure SQL-adatbázis (például egy webhely, amely tárolja a hello munkamenet-információk és adatok). hello második csak számítási-szolgáltatások (például egy Számológép vagy kép thumbnailing), amelyeket nem kezel olyan állandó állapotokat.

A vagy állapotmentes szolgáltatások particionálás esetben egy nagyon ritkán forgatókönyv – a méretezhetőség és rendelkezésre állási általában több példány hozzáadásával érhető el. kérelmek hello csak időtartamát tooconsider toomeet különleges útválasztási van szüksége több partíciót állapotmentes szolgáltatások-példányok esetén.

Tegyük fel fontolja meg egy olyan esetben, ha egy bizonyos tartomány-azonosítóval rendelkező felhasználók csak szolgáltatható által egy adott szolgáltatáspéldány. Ha sikerült partícióazonosító állapotmentes szolgáltatások egy másik példa, amikor egy valóban particionált háttér (pl. szilánkos SQL-adatbázis) rendelkezik, és azt szeretné, hogy mely szolgáltatáspéldány kell toohello adatbázis shard--írási vagy más belül előkészítő feladatok végrehajtására toocontrol hello állapotmentes szolgáltatások igénylő hello azonos partíciós információi szerint hello háttér használatban van. Ilyen típusú forgatókönyvek is különböző módon kell megoldani, és nem feltétlenül igényel szolgáltatás particionálást.

Ez a bemutató részében hello állapotalapú szolgáltatások összpontosít.

### <a name="partition-service-fabric-stateful-services"></a>A Service Fabric állapotalapú szolgáltatások partíció
A Service Fabric teszi könnyen toodevelop méretezhető állapotalapú szolgáltatások első osztályú úgy felajánlásával toopartition állapota (adatok). Fogalmilag, is gondol a partíció egy állapotalapú szolgáltatás, amely nagymértékben megbízható keresztül skálázási egységként [replikák](service-fabric-availability-services.md) , amely az elosztott és hello fürtben található csomópontok között.

A Service Fabric állapotalapú szolgáltatások hello környezetében particionálás hivatkozik toohello folyamat meghatározása, hogy egy adott szolgáltatáshoz partíció feladata hello hello szolgáltatás teljes állapota része. (Ahogy korábban említettük, a partíció egy olyan [replikák](service-fabric-availability-services.md)). A Service Fabric kiváló számítógépben, hogy különböző csomópontokon hello partíciók helyezi. Ez lehetővé teszi őket toogrow tooa csomópont erőforrás korlátját. Hello adatok igények nő, partíciók nő, és a Service Fabric partíciók-csomópontokon keresztüli újra egyensúlyba hozza.. Ez biztosítja, hogy hello további hardver-erőforrások hatékony használatát.

toogive például mondja meg az 5-csomópontból álló fürt és a beállított toohave 10 partíciók és három replikák célja egy szolgáltatás elindítása. Ebben az esetben a Service Fabric volna elosztása és hello replikák elosztják a hello fürt--és meg kellene végül két fő [replikák](service-fabric-availability-services.md) csomópontonként.
Ha most kell tooscale hello too10 fürtcsomópontok ki, a Service Fabric volna egyensúlyba hello elsődleges [replikák](service-fabric-availability-services.md) minden 10 csomópontra. Hasonlóképpen méretezhető csomópontja hátsó too5, ha a Service Fabric volna egyensúlyba összes hello replika hello 5-csomópont között.  

2. ábrán látható hello terjesztési 10 partíciók előtt és után hello fürt méretezése.

![Az állapotalapú szolgáltatás](./media/service-fabric-concepts-partitioning/partitions.png)

Ennek eredményeképpen hello kibővített érhető el, mert ügyfelek számítógépek különböző pontjain, hello alkalmazás általános teljesítmény akkor javul, és az adatok a hozzáférés toochunks versengés csökken.

## <a name="plan-for-partitioning"></a>Particionálás tervezése
A szolgáltatás üzembe, mielőtt mindig érdemes particionálási stratégia, amely szükséges tooscale kimenő hello. Különböző módja van, de ezek milyen hello alkalmazást tooachieve kell összpontosítania. Ez a cikk hello környezethez Mérlegeljük hello némelyike több fontos szempont.

Egy jó megoldás, toothink kapcsolatos hello struktúra, amelyet a particionált, hello első lépéseként toobe hello állapot.

Vegyünk egy egyszerű példa. Ha a szolgáltatás egy countywide lekérdezési toobuild, létrehozhat egy partíció mindegyik városhoz hello megye a. Ezt követően tárolhatja minden személy hello szavazatok hello városban, amely megfelel a toothat város hello partícióban. 3. ábra azt mutatja be, személyek és hello város, amelyben található készlete.

![Egyszerű partíció](./media/service-fabric-concepts-partitioning/cities.png)

Mivel hello feltöltési város széles körben változik, akkor előfordulhat, hogy végül néhány nagy mennyiségű adatot (pl. budapest) tartalmazó partíciókat és a többi partíció csekély állapotú (pl. Kirkland). De mi, hogy a partíciók állapot egyenetlen mennyiségű hello hatás?

Ha úgy gondolja, hogy kapcsolatos hello példa újra, könnyen láthatja, hogy hello partíció, amely tárolja a budapesti kvórumszavazatokat hello kap egy hello Kirkland-nál nagyobb forgalmat. Alapértelmezés szerint a Service Fabric gondoskodik arról, hogy nincs kapcsolatos hello azonos számú elsődleges és másodlagos replikák minden egyes csomóponton. Így használható a replikák átadott nagyobb forgalmat, míg mások kisebb forgalmat rendelkező csomópont. Lehetőleg érdemes tooavoid kiemelt és cold tesztüzeméhez, például a fürtben.

A rendezés tooavoid ezt, a particionálási szempontjából két dolgot kell tenni:

* Próbálja toopartition hello állapotát, hogy az összes partíciójára egyenletesen vannak elosztva.
* Az egyes hello replikák hello szolgáltatás betöltésének jelentéséhez. (Kapcsolatban tekintse meg a cikk a [metrikák és a betöltés](service-fabric-cluster-resource-manager-metrics.md)). A Service Fabric biztosítja hello funkció tooreport hálózati szolgáltatások, például a memória vagy a rekordok száma használni. Jelentett hello mérőszámok alapján, a Service Fabric észleli, hogy az egyes partíciók többinél magasabb terhelés szolgál, és újra egyensúlyba hozza a mozgóátlag replikák toomore megfelelő csomópontok, hello fürt teljes nincs csomópont túl van terhelve.

Néha nem tudja, mennyi adatot lesznek az egyes partíciók eseménysorozatában. Így egy általános javaslat toodo mindkét--először jó particionálási stratégia elfogadásával, amelyek között osztja el hello adatok egyenletesen hello partíciókat és a második, jelentéskészítési terhelés által.  első módszer hello megakadályozza, hogy a példában szavazás során hello második segítségével zökkenőmentes hozzáférést vagy terheléselosztási ideiglenes különbségeit kimenő időbeli hello helyzet.

Egy másik partíció tervezési célja toochoose hello számát a partíciók toobegin.
A Service Fabric szempontjából nincs szükség, amely megakadályozza, hogy kezdte meg az magasabb számú partíciót adott esetben a vártnál.
Feltéve, hogy hello több partíció valójában egy érvényes megközelítés.

Ritka esetekben használható kellene kezdetben kiválasztott számánál több partíciót. Hello partíciószám után hello tény nem módosítható, mert kellene tooapply néhány speciális partíció módszerek, például létrehozhat egy új szolgáltatás példányának hello azonos szolgáltatás típusa. Módosítania kell tooimplement néhány ügyféloldali logika, amely továbbítja a hello kérelmek toohello megfelelő szolgáltatáspéldány, az Ügyfélkód kell karbantartani ügyféloldali ismeretek alapján.

Meg kell vizsgálni a particionálás tervezési, hello elérhető számítógép-erőforrásokat. Hello állapot igények toobe érhető el, és tárolja, kötött toofollow áll:

* Hálózati sávszélesség korlátja
* Rendszer memóriakorlátokat
* Lemez tárolási korlátai

Így mi történik, ha egy futó fürt erőforrás korlátokat futtatja? hello választ ki, hogy egyszerűen méretezheti hello fürt tooaccommodate hello új követelményeket kell.

[kapacitástervezési útmutató hello](service-fabric-capacity-planning.md) útmutatást kínál toodetermine hány csomópontokat a fürthöz kell.

## <a name="get-started-with-partitioning"></a>Ismerkedés a particionálás
Ez a szakasz ismerteti, hogyan tooget a szolgáltatás particionálás használatába.

A Service Fabric kiválaszthatja, hogy három partíciós séma kínálja:

* Címkiosztási particionálás (más néven UniformInt64Partition).
* Nevű particionálást. Ez a modell általában használó alkalmazások is lehet bucketed, a kötött belül adatokkal rendelkeznek. Néhány gyakori példán elnevezett partíciókulcsok használt adatmezők lenne, régiók, postai, felhasználói csoportok vagy egyéb üzleti határokat.
* Particionálás egypéldányos. Hello szolgáltatást nem igényel további útválasztási egypéldányos partíciók általában használják. Például a állapotmentes szolgáltatásokhoz használja ezt a particionálási sémát alapértelmezés szerint.

Nevű és egypéldányos particionálási sémák a következők: címkiosztási partíciók speciális formája. Alapértelmezés szerint a Service Fabric használatra hello Visual Studio sablonok címkiosztási particionálás, mert az hello közös és a hasznos egy. hello a cikk hátralévő része hello címkiosztási particionálási sémát összpontosít.

### <a name="ranged-partitioning-scheme"></a>Címkiosztási particionálási sémát
Ez az egész használt toospecify (azonosított egy kis és nagy kulcsot) tartomány- és a partíciók (n) száma. N partíciók, minden egyes felelős a hello egy mozaikként, átfedés nélkül alosztály hoz létre teljes partícióazonosító kulcs tartományon. Például egy ranged particionálási sémát 0 alacsony kulccsal, 99 magas kulcs, és a 4 számát hozna létre négy partíciót alább látható módon.

![Particionálás között](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Általános gyakorlatként javasolt toocreate hello adatkészlet belül egyedi kulcs alapján kivonatát. Néhány gyakori példán kulcsok lenne, a vehicle azonosító szám (VIN), az alkalmazott azonosítója vagy egy egyedi karakterlánc. Az egyedi kulcs használatával, hoz majd létre egy kivonatoló modulus hello kulcs tartomány, a toouse a kulcsként. Hello felső és alsó határainak hello megengedett kulcs tartományt is megadhat.

### <a name="select-a-hash-algorithm"></a>A kivonatoló algoritmus kiválasztása
A kivonatolás fontos része a kivonatoló algoritmus kijelölése. Egy kell vizsgálni, hogy-e hello cél toogroup hasonló kulcsok egymást (helység bizalmas kivonatoláshoz) –, vagy ha körben tevékenységet kell terjeszteni összes partíciójára (terjesztési kivonatoláshoz), amely napjainkban egyre általánosabbá.

hello jó terjesztési kivonatoló algoritmus mutatókat, hogy könnyen toocompute, van néhány ütközések, és egyenletesen terjesztett hello kulcsok. Egy jó példa egy hatékony kivonatoló algoritmust, hogy hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) kivonatoló algoritmus.

Általános kivonatoló kódot algoritmus lehetőségekért helyes erőforrás hello [Wikipedia oldalon, kivonatoló függvényeket](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Több partíciót az állapotalapú szolgáltatás létrehozása
Hozzuk létre az első megbízható állapotalapú szolgáltatás több partíciókat. Ebben a példában fog létrehozni egy nagyon egyszerű alkalmazás toostore kívánt összes utolsó neve az azonos hello a levél hello egyazon partícióra kerüljenek.

Kód írása előtt kell toothink hello partíciókról és partíciós kulcsok. Partíciókra van szüksége 26 (egy hello ábécé minden betű), de mi kapcsolatos alacsony és magas kulcsok hello?
Szeretnénk szó toohave egy partíciót engedélyez betű, azt használatával 0 hello alsó kulcsa és 25 kulcsként hello magas, mert mindegyik betűnek a saját kulcs.

> [!NOTE]
> Ez egy webfarmos egyszerűsített, valójában hello terjesztési egyenetlen lenne. Gyakori hello állók közül. "X" kezdő "S" vagy "M" hello betűket kezdve Vezetéknév vagy az "Y".
> 
> 

1. Nyissa meg **Visual Studio** > **fájl** > **új** > **projekt**.
2. A hello **új projekt** párbeszédpanelen válassza ki a Service Fabric-alkalmazás hello.
3. Hello projekt "AlphabetPartitions" hívása.
4. A hello **szolgáltatás létrehozása** párbeszédpanelen válassza ki **állapotalapú alkalmazások és szolgáltatások** szolgáltatást, és az "Alphabet.Processing" hívás a hello az alábbi képen látható módon.
       ![Visual Studio új szolgáltatás párbeszédpanelje][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. A partíciók számának hello beállítása. Nyissa meg hello Applicationmanifest.xml fájl található hello ApplicationPackageRoot mappa hello AlphabetPartitions projektet és a frissítés hello paraméter Processing_PartitionCount too26 alább látható módon.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Szükség tooupdate hello LowKey és HighKey tulajdonságok hello StatefulService elemének hello ApplicationManifest.xml alább látható módon.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Hello szolgáltatás toobe érhető el nyissa meg egy portot a végpont mentése hello végpontelem ServiceManifest.xml (hello PackageRoot mappában található), az alább látható módon Alphabet.Processing szolgáltatás hello hozzáadásával:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Hello szolgáltatás most konfigurált toolisten tooan belső végpont 26 partíciókkal.
7. A következő lépésben toooverride hello `CreateServiceReplicaListeners()` hello feldolgozási osztály.
   
   > [!NOTE]
   > Ebben a példában feltételezzük, hogy egy egyszerű HttpCommunicationListener használunk. A megbízható szolgáltatás kommunikációja további információkért lásd: [hello megbízható kommunikáció modell](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Az ajánlott mintázatát hello URL-címet, amely figyeli a replika hello a következő formátumban: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Ezért érdemes tooconfigure a kommunikációs figyelő toolisten hello megfelelő végpontok, hogy az ebben a mintában.
   
    Előfordulhat, hogy a szolgáltatás több replika futó hello ugyanazon a számítógépen, ezért ezt a címet kell toobe egyedi toohello replika. Ezért Partícióazonosító + másodpéldány-azonosító van hello URL-címben. HttpListener figyelheti az azonos port mindaddig, amíg hello URL-előtagját egyedi hello több címen.
   
    hello extra GUID van-e egy speciális eset, amelyen másodlagos replika is a kéréseket kell figyelnie csak olvasható. Amikor hello esetben szüksége arra, hogy egy új egyedi címet használja, amikor elsődleges toosecondary tooforce ügyfelek toore feloldása hello cím átállás toomake. "+ a" hello címet, hogy hello replika figyeli az összes elérhető gazdagépet (IP, FQDM, localhost, stb.) hello kódot mutat egy példát használatos.
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Célszerű is érdemes megjegyezni, hogy hello közzétett URL-címe eltér némileg hello figyelő URL-előtagját.
    tooHttpListener hello figyelő URL-címet kap. közzétett URL-címe: hello URL-címet, amely közzétett toohello Service Fabric-szolgáltatás, a szolgáltatásészlelés használt hello. Az ügyfelek ekkor megkérdezi, ehhez a címhez, a felderítés szolgáltatáson keresztül. hello címét, hogy az ügyfelek igényeinek toohave hello tényleges IP vagy FQDN-jét hello csomópont beolvasni a rendelés tooconnect. Ezért meg kell tooreplace "+" rendelkező hello csomópont IP-cím vagy FQDN látható a fenti.
9. hello utolsó lépése tooadd hello feldolgozási logika toohello szolgáltatás alább látható módon.
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest`olvasási hello hello lekérdezési karakterlánc paraméter használt toocall hello partíció és hívások értékének `AddUserAsync` tooadd hello Vezetéknév toohello megbízható szótár `dictionary`.
10. Adjuk hozzá egy állapotmentes szolgáltatások toohello projekt toosee hogyan hívása egy adott partíció.
    
    Ez a szolgáltatás egyszerű webes felületet, amely elfogadja a lekérdezési karakterlánc paraméterként hello Vezetéknév, hello partíciós kulcs határozza meg, és elküldi toohello Alphabet.Processing szolgáltatás feldolgozásra funkcionál.
11. A hello **szolgáltatás létrehozása** párbeszédpanelen válassza ki **Stateless** szolgáltatás, és hívja meg az "Alphabet.Web" alább látható módon.
    
    ![Állapotmentes szolgáltatások képernyőképe](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Hello végpont-információ frissítésére hello ServiceManifest.xml a hello Alphabet.WebApi szolgáltatás tooopen be a portot a lent látható módon.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Tooreturn hello osztályban webes ServiceInstanceListeners gyűjteménye van szüksége. Ebben az esetben választható tooimplement egy egyszerű HttpCommunicationListener.
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. Most tooimplement hello feldolgozó logika van szüksége. hello HttpCommunicationListener hívások `ProcessInputRequest` Ha kérelem érkezik. Ezért lépjen tovább, és adja hozzá az alábbi hello kódot.
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Rajta lépésről lépésre bemutatjuk. hello kód beolvassa hello lekérdezési karakterlánc paraméter első betűjének hello `lastname` történő karakter lehet. Majd, meghatározza, hogy ez a levél hello partíciókulcs hexadecimális értékét hello kivonásával `A` hello hexadecimális értékét hello Vezetéknév első betűjét.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Ne feledje, hogy a jelen példában használjuk 26 partíciók partíciónként több partíciós kulccsal.
    A következő azt beszerzése hello szolgáltatás partíció `partition` hello segítségével a kulcs `ResolveAsync` hello metódusa `servicePartitionResolver` objektum. `servicePartitionResolver`típusúként van definiálva
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    Hello `ResolveAsync` metódust vesz hello szolgáltatás URI-azonosítója, hello partíciókulcs és paraméterekként cancellation jogkivonatot. hello szolgáltatás URI-azonosítója a feldolgozási szolgáltatás hello `fabric:/AlphabetPartitions/Processing`. A következő azt lekérése hello végpont hello partíció.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Végül azt hello végpont URL-cím és hello lekérdezési karakterlánc felépítéséhez, és hívja service feldolgozása hello.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    Ha hello feldolgozási végzett, azt visszaírni hello kimeneti.
15. hello utolsó lépés egy tootest hello szolgáltatás. A Visual Studio által használt alkalmazás paramétereket a helyi és a felhő üzembe helyezése. tootest hello szolgáltatás helyileg 26 partíciókkal van szüksége tooupdate hello `Local.xml` fájl hello AlphabetPartitions projekt hello ApplicationParameters mappában a lent látható módon:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Miután befejezte a központi telepítés, ellenőrizheti a hello szolgáltatás és a Service Fabric Explorer hello a partíciók száma.
    
    ![Service Fabric Explorer képernyőképe](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. A böngészőben, tesztelheti a particionálás logika megadásával hello `http://localhost:8081/?lastname=somename`. Látni fogja, hogy minden egyes utolsó, amely kezdetű névvel rendelkező azonos levél tárolása az hello hello egyazon partícióra kerüljenek.
    
    ![Böngésző képernyőképe](./media/service-fabric-concepts-partitioning/samplerunning.png)

hello teljes forráskód hello minta nem érhető el a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="next-steps"></a>Következő lépések
A Service Fabric fogalmakat további információkért lásd: hello következő:

* [A Service Fabric-szolgáltatások rendelkezésre állása](service-fabric-availability-services.md)
* [Méretezhetőséget biztosít a Service Fabric-szolgáltatások](service-fabric-concepts-scalability.md)
* [Kapacitástervezés a Service Fabric-alkalmazások](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png