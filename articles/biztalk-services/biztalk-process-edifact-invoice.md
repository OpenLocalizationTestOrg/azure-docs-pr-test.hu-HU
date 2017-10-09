---
title: "Oktatóanyag: Azure BizTalk szolgáltatásokkal EDIFACT számlák feldolgozásához |} Microsoft Docs"
description: "Hogyan toocreate és hello mezőben összekötő vagy API-alkalmazások konfigurálása, és felhasználhatja őket a logikai alkalmazás az Azure App Service-ben"
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 4ca021c9084b9b6540c93ef15fc3d44be0966970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Oktatóanyag: Folyamat EDIFACT számlák Azure BizTalk szolgáltatás használata

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk szolgáltatások portálja tooconfigure hello használja, és X12 és EDIFACT-egyezmények telepítése. Ebben az oktatóanyagban úgy tekintünk, hogyan toocreate cseréjét EDIFACT megállapodást számlák üzleti partnerek között. Ez az oktatóanyag egy végpont megoldás használata esetén két kereskedelmi partnereknek, EDIFACT üzeneteket Northwind és Contoso körül írása.  

## <a name="sample-based-on-this-tutorial"></a>Ez az oktatóanyag alapján minta
Ez az oktatóanyag egy minta körül írása **küldése EDIFACT számlák BizTalk szolgáltatások használatával**, vagyis hello a rendelkezésre álló toodownload [MSDN Kódgalériából](http://go.microsoft.com/fwlink/?LinkId=401005). Nem sikerült hello mintát használja, és ezen oktatóanyag toounderstand keresztül halad, hogyan hello minta lett létrehozva. Vagy az oktatóanyag toocreate használhatja a saját megoldás ground felfelé. Ez az oktatóanyag hello második megközelítés cél, hogy megismerte a hogyan készült el ebben a megoldásban. Is lehetőség szerint hello oktatóanyag konzisztens hello példával, és használja ugyanazokat az összetevők (például sémákat, átalakítások) hello mintában használt neveket hello.  

> [!NOTE]
> Mivel ezen megoldás keretein üzenet küldése egy EAI-Összekötők híd tooan EDI híd, akkor a rendszer újból felhasználja hello [BizTalk szolgáltatások híd minta láncolás](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) minta.  
> 
> 

## <a name="what-does-hello-solution-do"></a>Mire hello megoldás?
Ebben a megoldásban a Northwind EDIFACT számlák megkapja a Contoso. A következő számlák nincsenek szabványos EDI-formátumú. Igen hello számla tooNorthwind, mielőtt az átalakított tooan EDIFACT számla (más néven INVOIC) dokumentumnak kell lennie. Fogadását, a Northwind kell hello EDIFACT számla feldolgozni, és térjen vissza a vezérlő (más néven CONTRL) üzenet tooContoso.

![][1]  

tooachieve a üzleti forgatókönyv, a Contoso használja a Microsoft Azure BizTalk szolgáltatások által nyújtott hello szolgáltatásokat.

* A Contoso EAI-Összekötők hidak tootransform hello eredeti számla tooEDIFACT INVOIC használ.
* hello EAI-Összekötők híd ezután elküldi a hello üzenet tooan EDI telepített híd küldése hello BizTalk szolgáltatások portálja konfigurált szerződés részeként.
* hello EDI küldési híd hello EDIFACT INVOIC folyamatokat, és továbbítja azt tooNorthwind.
* Hello számla beérkezése után a Northwind egy CONTRL üzenet toohello EDI kap híd hello szerződés részeként adja vissza.  

> [!NOTE]
> Szükség esetén ez a megoldás is bemutatja, hogyan toosend hello kötegelés toouse számlák kötegekben, minden egyes számla külön küldése helyett.  
> 
> 

toocomplete hello esetben használjuk a Service Bus-üzenetsorok toosend Contoso tooNorthwind át, vagy fogadnak nyugtázási Northwind. Ezek a várólisták hozhatók létre egy ügyfélalkalmazást, amely letölthető, és elérhető hello minta csomagban található ebben az oktatóanyagban részeként.  

## <a name="prerequisites"></a>Előfeltételek
* A Service Bus-névtér kell rendelkeznie. Egy névtér létrehozása vonatkozó utasításokért lásd: [Útmutató: létrehozhat vagy módosíthat egy Service Bus szolgáltatás Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Tételezzük fel, hogy már rendelkezik kiépített, Service Bus-névtér neve **edifactbts**.
* A BizTalk szolgáltatások előfizetéssel kell rendelkeznie. Útmutatásért lásd: [klasszikus Azure portál használatával BizTalk szolgáltatás létrehozása](http://go.microsoft.com/fwlink/?LinkID=302280). Ebben az oktatóanyagban Tételezzük nevű BizTalk szolgáltatások előfizetéssel rendelkezik **contosowabs**.
* BizTalk szolgáltatások előfizetését a BizTalk szolgáltatások portálja hello regisztrálni. Útmutatásért lásd: [regisztrálása a BizTalk szolgáltatás központi telepítés hello BizTalk szolgáltatások portálja](https://msdn.microsoft.com/library/hh689837.aspx)
* Visual Studio telepítve kell rendelkeznie.
* BizTalk szolgáltatások SDK telepítve kell rendelkeznie. Letöltheti az SDK hello [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-hello-service-bus-queues"></a>1. lépés: Hello Service Bus-üzenetsorok létrehozása
Ez a megoldás a Service Bus üzenetsorok tooexchange üzenetek között kereskedelmi partnereknek használ. A Contoso és a Northwind üzeneteket küldhet toohello várólisták a ahol hello EAI-Összekötők és/vagy EDI hidak felhasználni azokat. Ez a megoldás három Service Bus-üzenetsorok szükséges:

* **northwindreceive** – Northwind fogad hello számla Contoso keresztül ebből a várólistából.
* **contosoreceive** – Contoso fogad hello nyugtázási Northwind keresztül ebből a várólistából.
* **Felfüggesztve** – az összes felfüggesztett üzenet irányított toothis várólista. Üzenetek fel vannak függesztve, ha azt elmulasztják feldolgozása során.

A Service Bus-üzenetsorok hozhat létre egy ügyfélalkalmazást, hello minta csomag használatával.  

1. Hello minta kezelőportálon hello helyről nyissa meg a **oktatóanyag küldése számlák használatával BizTalk szolgáltatások EDI Bridges.sln**.
2. Nyomja le az **F5** toobuild és kezdő hello **oktatóanyag ügyfél** alkalmazás.
3. Az üdvözlő képernyőt adja meg a hello Service Bus ACS névteret, a kibocsátó neve és a kibocsátó kulcsa.
   
   ![][2]  
4. Egy üzenetablak megadását kéri, hogy a Service Bus-névtér három várólisták létrejön. Kattintson az **OK** gombra.
5. Hagyja hello oktatóanyag ügyfél futnak. Nyissa meg a hello, kattintson **Service Bus** > ***a Service Bus-névtér*** > **várólisták**, és győződjön meg arról, hogy három hello várólisták jöttek létre.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>2. lépés: Hozzon létre, és kereskedelmipartner-egyezmény telepítése
A Contoso és a Northwind közötti kereskedelmipartner-egyezmény létrehozása. Kereskedelmipartner-egyezmény definiál egy kereskedelmi szerződés között két hello üzleti partnerek, mely üzenet séma toouse, például amelyek üzenetküldési protokoll toouse stb. Kereskedelmipartner-egyezmény két EDI hidak tartalmaz, akkor egy toosend üzenetek tootrading partnerek (hello nevű **EDI küldése híd**) és egy tooreceive üzenetek kereskedelmi partnerek (hello nevű **EDI-fogadási híd**).

Az ilyen típusú megoldásra hello környezetben hello EDI küldési híd megfelel toohello küldési-oldalon hello megállapodás és használt toosend hello EDIFACT számla a Contoso tooNorthwind. Hasonlóképpen hello EDI kap híd toohello fogadóoldali hello megállapodás megfelel és használt tooreceive nyugtázása a Northwind.  

### <a name="create-hello-trading-partners"></a>Kereskedelmi partnerek hello létrehozása
toostart rendelkező, kereskedelmi partnereknek a Contoso és a Northwind hozzon létre.  

1. A BizTalk szolgáltatások portálon hello a hello **partnerek** lapra, majd **Hozzáadás**.
2. Hello új partner lapon adja meg a **Contoso** lehetőséget egy partner neve, majd kattintson **mentése**.
3. Ismétlődő hello lépés toocreate hello második partner, **Northwind**.  

### <a name="create-hello-agreement"></a>Hello-egyezmény létrehozása
Kereskedelmipartner-egyezmények üzleti profilok kereskedelmi partnerek között jönnek létre. Ez a megoldás hello alapértelmezett partner profilok jönnek létre automatikusan létrehozott hello partnerek használ.  

1. BizTalk szolgáltatások portálja hello, kattintson **megállapodások** > **Hozzáadás**.
2. Hello új szerződésben **általános beállítások** lapon hello értékeket megadni, az alábbi hello ábrán látható módon, és kattintson a **Folytatás**.
   
   ![][3]  
   
   Miután rákattintott **Folytatás**, két lap található, **fogadási beállítások** és **küldési beállítások** elérhetővé válnak.
3. Hozzon létre hello küldési megállapodás Contoso és a Northwind között. Ez a szerződés szabályozza, hogyan Contoso küldi hello EDIFACT számla tooNorthwind.
   
   1. Kattintson a **küldési beállításainak**.
   2. Tartsa meg az alapértelmezett értékét hello hello **bejövő URL**, **átalakítási**, és **Batching** lapokon.
   3. A hello **protokoll** lap hello **sémák** részen töltse fel a hello **EFACT_D93A_INVOIC.xsd** séma. Ebben a sémában hello minta csomaggal érhető el.
      
      ![][4]  
   4. A hello **átviteli** lapra, adja meg a Service Bus-üzenetsorok hello hello adatait. Hello küldési-oldalon szerződés, hello használjuk **northwindreceive** toosend hello EDIFACT számla tooNorthwind várólistára, és hello **felfüggesztve** várólistára tooroute üzenetek, és a feldolgozás során sikertelen felfüggesztve. Ezek a várólisták a létrehozott **1. lépés: hello létrehozása a Service Bus-üzenetsorok** (szakaszát).
      
      ![][5]  
      
      A **átviteli beállításai > átviteli típus** és **felfüggesztéséről az Üzenetbeállítások > átviteli típus**Azure Service Bus válassza ki, és adja meg hello hello ábrán látható módon.
4. Hozzon létre hello fogadni a Contoso és a Northwind között. Ez a szerződés szabályozza, hogyan Contoso Northwind hello nyugtázási kap.
   
   1. Kattintson a **beállítások**.
   2. Tartsa meg az alapértelmezett értékét hello hello **átviteli** és **átalakítási** lapokon.
   3. A hello **protokoll** lap hello **sémák** részen töltse fel a hello **EFACT_4.1_CONTRL.xsd** séma. Ebben a sémában hello minta csomaggal érhető el.
   4. A hello **útvonal** lap, hozzon létre egy szűrő tooensure, amely csak a nyugtázás a Northwind irányított tooContoso. A **útvonal beállítások**, kattintson a **Hozzáadás** toocreate hello útválasztási szűrő.
      
      ![][6]  
      
      1. Adjon meg értékeket a **szabálynév**, **útvonal szabály**, és **útvonal cél** hello ábrán látható módon.
      2. Kattintson a **Save** (Mentés) gombra.
   5. A hello **útvonal** újra lapra, adja meg, ahol felfüggesztett nyugták (a nyugtázás a feldolgozás során eleget nem tevő) legyenek átirányítva. Hello átviteli típus tooAzure Service Bus beállításához céltípus útvonal túl**várólista**, hitelesítési írja be a túl**közös hozzáférésű Jogosultságkód** (SAS), a Service Bus hello hello SAS kapcsolati karakterláncot adja meg névtér, majd adja meg a hello várólista neve megegyezik **felfüggesztve**.
5. Végezetül kattintson **telepítés** toodeploy hello szerződést. Megjegyzés: hello végpontok, ahol hello küldeni és fogadni megállapodások telepítve.
   
   * A hello **küldési beállítások** lap **bejövő URL**, vegye figyelembe a hello végpont. toosend EDI küldése híd hello segítségével Contoso tooNorthwind üzenetét, el kell küldenie egy üzenet toothis végpontot.
   * A hello **fogadási beállítások** lap **átviteli**, vegye figyelembe a hello végpont. Northwind tooContoso hello EDI használatával üzenetét toosend kap híd, el kell küldenie egy üzenet toothis végpontot.  

## <a name="step-3-create-and-deploy-hello-biztalk-services-project"></a>3. lépés: Hozzon létre és hello BizTalk szolgáltatások projekt telepítése
Hello előző lépésben telepített hello EDI és küldésére és fogadására megállapodások tooprocess EDIFACT számlák nyugták. Ezek a szerződések csak toohello szabványos EDIFACT üzenet séma tartó folyamatot üzeneteket is. Azonban / hello forgatókönyv ebben a megoldásban, Contoso küld egy számla tooNorthwind belső fejlesztésű jogvédett sémában. Igen előtt EDI küldése híd toohello hello üzenetet küld, azt kell alakul hello belső fejlesztésű séma toohello szabványos EDIFACT számla séma. hello BizTalk EAI-szolgáltatások Projekt funkciója, amely.

BizTalk szolgáltatások projektben hello **InvoiceProcessingBridge**, hogy átalakítások hello üzenet egyben tartalmazzák a letöltött hello minta. hello projekt tartalmazza a következő összetevők hello:

* **INHOUSEINVOICE. XSD** – hello belső fejlesztésű számla tooNorthwind küldött séma.
* **EFACT_D93A_INVOIC. XSD** – hello szabványos EDIFACT számla séma.
* **EFACT_4.1_CONTRL. XSD** –, hogy a Northwind tooContoso küldi hello EDIFACT nyugtázási sémája.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – hello átalakító, amely leképezhető hello belső fejlesztésű számla séma toohello szabványos EDIFACT számla séma.  

### <a name="create-hello-biztalk-services-project"></a>Hello BizTalk szolgáltatások projekt létrehozása
1. A Visual Studio megoldás hello, bontsa ki hello InvoiceProcessingBridge projektet, és nyissa meg a hello **MessageFlowItinerary.bcs** fájlt.
2. Kattintson bárhová a vásznon a hello és hello beállítása **BizTalk szolgáltatás URL-címe** a hello tulajdonság mezőben toospecify a BizTalk szolgáltatások előfizetés nevét. Például: `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Hello eszközkészletről húzzon egy **Xml One-Way híd** toohello vászonra. Set hello **egyednév** és **relatív címet** hello tulajdonságainak híd túl**ProcessInvoiceBridge**. Kattintson duplán a **ProcessInvoiceBridge** tooopen hello híd konfigurációs felületét.
4. Hello belül **üzenettípusok** mezőben, kattintson a plusz hello (**+**) gomb toospecify hello séma hello bejövő üzenet. Mivel hello bejövő üzenete hello EAI-Összekötők híd mindig hello belső fejlesztésű számla, állítsa túl**INHOUSEINVOICE**.
   
   ![][8]  
5. Hello kattintson **XML-átalakító** alakzat, és a hello tulajdonság mezőben a hello **Maps** tulajdonság, hello három pont gombra (**...** ) gombra. A hello **Maps kijelölés** párbeszédpanel megnyitásához, jelölje be hello **INHOUSEINVOICE_to_D93AINVOIC** fájl átalakítása, és kattintson a **OK**.
   
   ![][9]  
6. Lépjen vissza túl**MessageFlowItinerary.bcs**, és hello eszközkészletről húzzon egy **kétirányú külső végpont** toohello sarkában hello **ProcessInvoiceBridge**. Állítsa be a **egyednév** tulajdonság túl**EDIBridge**.
7. A Solution Explorer hello, bontsa ki a hello **MessageFlowItinerary.bcs** , és kattintson duplán a hello **EDIBridge.config** fájlt. Cserélje le a hello hello tartalmának **EDIBridge.config** hello következőre.
   
   > [!NOTE]
   > Miért kell tooedit hello .config fájl? azt hozzáadásának toohello híd tervezői vásznon a hello külső szolgáltatási végpont hello EDI hidak korábbi helyezett jelöli. EDI hidak kétirányú hidak, a Küldés és a fogadási oldalon. Hello EAI-Összekötők híd, hogy toohello híd designer hozzáadott azonban egy egyirányú híd. Igen toohandle hello másik üzenet exchange mintákat keressen az hello két hidak, használjuk egy egyéni híd viselkedés konfigurációjában belefoglalja hello .config kiterjesztésű fájl. Emellett a hello egyéni viselkedés is kezeli a hello hitelesítési toohello EDI küldési híd végpontot. Ez a viselkedés egyéni áll rendelkezésre, mert egy külön mintát [BizTalk szolgáltatások híd minta - EAI-Összekötők tooEDI láncolás](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Ez a megoldás a rendszer újból felhasználja hello minta.  
   > 
   > 
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.serviceModel>
       <extensions>
         <behaviorExtensions>
           <add name="BridgeAuthentication" 
                 type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
         </behaviorExtensions>
       </extensions>
       <behaviors>
         <endpointBehaviors>
           <behavior name="BridgeAuthenticationConfiguration">
             <!-- Enter hello ACS namespace, issuer name and issuer secret of hello BizTalk Services deployment -->
             <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                   issuername="owner" 
                                   issuersecret="[YOUR ACS SECRET]" />
             <webHttp />
           </behavior>
         </endpointBehaviors>
       </behaviors>
       <bindings>
         <webHttpBinding>
           <binding name="BridgeBindingConfiguration">
             <security mode="Transport" />
           </binding>
         </webHttpBinding>
       </bindings>
       <client>
         <clear />
         <!--
           Go BizTalk Portal > Agreement > Send Settings > Inbound URL
           Copy hello Endpoint URL and paste it in hello below address field
         -->
         <endpoint name="TwoWayExternalServiceEndpointReference1" 
                   address="[YOUR EDI BRIDGE SEND URI]" 
                   behaviorConfiguration="BridgeAuthenticationConfiguration" 
                   binding="webHttpBinding" 
                   bindingConfiguration="BridgeBindingConfiguration" 
                   contract="System.ServiceModel.Routing.IRequestReplyRouter" />
       </client>
     </system.serviceModel>
   </configuration>
   
   ```
8. Hello EDIBridge.config fájl tooinclude konfigurációs részleteinek frissítése
   
   * A  *<behaviors>* , adja meg az ACS-névtér hello és hello BizTalk-Services-előfizetésével társított kulcs.
   * A  *<client>* , adja meg a hello végpont EDI szerződés küldése hello telepítési helyét.
   
   Mentse a módosításokat, és zárja be a hello konfigurációs fájl.
9. A hello eszközkészlet, kattintson a hello **összekötő** és illesztési hello **ProcessInvoiceBridge** és **EDIBridge** összetevőket. Válassza ki hello összekötő, és a Tulajdonságok párbeszédpanelen **szűrési feltételt** túl**egyezés minden**. Ez biztosítja, hogy az összes hello EAI-Összekötők híd által feldolgozott üzenetek útválasztása toohello EDI híd.
   
   ![][10]  
10. Módosítások mentése toohello megoldás.  

### <a name="deploy-hello-project"></a>Hello projekt telepítése
1. Hello számítógép, amelyen létrehozta hello BizTalk szolgáltatások projekt töltse le és telepítse a BizTalk szolgáltatások előfizetését hello SSL-tanúsítványa. A, a BizTalk szolgáltatások területen kattintson **irányítópult**, és kattintson a **SSL-tanúsítvány letöltése**. Kattintson duplán a hello tanúsítványt, és kövesse a hello Rákérdezés toocomplete hello telepítését. Ellenőrizze, hogy a hello tanúsítványát telepítenie **megbízható legfelső szintű hitelesítésszolgáltatók** tanúsítványtárolójába.
2. A Visual Studio Solution Explorerben kattintson a jobb gombbal hello **InvoiceProcessingBridge** projektre, és kattintson a **telepítés**.
3. Adja meg hello hello ábrának megfelelően, és kattintson a **telepítés**. Kaphat hello ACS hitelesítő adatok BizTalk szolgáltatások kattintva **kapcsolatadatok** hello BizTalk szolgáltatások irányítópulton.
   
   ![][11]  
   
   Hello tesztkimenet ablaktáblán, másolja a hello EAI-Összekötők híd telepítési helyét, például hello végpont `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Később szüksége lesz a végponti URL-cím.  

## <a name="step-4-test-hello-solution"></a>4. lépés: Hello megoldás tesztelése
Ebben a témakörben úgy tekintünk, hogyan tootest hello hello segítségével megoldás **oktatóanyag ügyfél** alkalmazás hello minta részeként.  

1. A Visual Studio, nyomja le az F5 toostart hello **oktatóanyag ügyfél**.
2. üdvözlő képernyőt értéknek kell lennie hello hello lépésben előre feltöltve ahol létrehozott hello Service Bus-üzenetsorok. Kattintson a **Tovább** gombra.
3. Hello következő ablakban ACS hitelesítő adatok megadása BizTalk szolgáltatások előfizetésébe, és végpontok hello ahol EAI- és EDI (kap) hidak vannak telepítve.
   
   Hello előző lépésben másolt kellett hello EAI-Összekötők híd végpont. EDI híd végpont hello BizTalk szolgáltatások portálja kap, a go toohello megállapodás > fogadási beállítások > átviteli > végpont.
   
   ![][12]  
4. Hello következő ablakban, a Contoso, kattintson a hello **belső fejlesztésű számla küldése** gombra. A fájl hello párbeszédpanel megnyitásához, hello INHOUSEINVOICE.txt fájl megnyitásához. Vizsgálja meg a hello hello fájl tartalmát, és kattintson a **OK** toosend hello számla.
   
   ![][13]  
5. Néhány másodperc hello a számla Northwind fogadja. Kattintson a hello **nézet üzenet** hivatkozás toosee hello számla Northwind által fogadott. Figyelje meg, hogyan Northwind által fogadott hello számla közben hello egy Contoso által küldött szabványos EDIFACT séma van volt egy belső fejlesztésű séma.
   
   ![][14]  
6. Válassza ki a hello számla, és kattintson a **küldése nyugtázási**. Hello párbeszédpanelen ugrik fel figyelje meg, hogy hello interchange azonosítója legyen, a hello kapott számlán és hello nyugtázási küldi el. Kattintson az OK gombra a hello **küldése nyugtázási** párbeszédpanel megnyitásához.
   
   ![][15]  
7. Néhány másodpercen belül hello nyugtázási sikeresen Contoso fogadja.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>(Választható) 5. lépés: küldése EDIFACT számla kötegek
BizTalk szolgáltatások EDI hidak is támogatja a kimenő üzenetek kötegelés. Ez a szolgáltatás akkor hasznos, partnerek, amelyeknek tooreceive az üzenetkötegek (bizonyos feltételeknek megfelelő) helyett az egyes üzenetek fogadására.

hello egyik legfontosabb szempont az kötegek használatakor nem hello tényleges hello köteg, más néven hello kiadási feltételek. hello kiadási feltételek alapulhat hogyan hello fogadó partner szeretné-e tooreceive üzeneteket. Ha kötegelés engedélyezve van, hello EDI híd nem küldi hello partner fogadását, amíg hello kiadás feltétel teljesül, a kimenő üzenet toohello. Például egy kötegelési feltételek alapján üzenet mérete kivételkezelési egy kötegben csak akkor, ha "n" üzenetek kötegelni vannak. A kötegelt feltételeket is időalapú, úgy, hogy az adott időpontban naponta egy kötegelt zajlik. Ebben a megoldásban a Microsoft hello üzenetméret alapú feltételek próbálja meg.

1. BizTalk szolgáltatások portálja hello kattintson a korábban létrehozott hello szerződést. Kattintson a Küldés beállítások > kötegelés > kötegelt hozzáadása.
2. Kötegelt neveként, írja be a **InvoiceBatch**, adjon meg egy leírást, és kattintson a **következő**.
3. Adjon meg egy kötegelt feltételeket, amely meghatározza, hogy mely üzenetek kötegelni kell lennie. Ebben a megoldásban kötegelt azt minden üzenetet. Igen, válassza ki a hello speciális definíciók beállítás használatát, és adja meg **1 = 1**. Ez egy feltételt, amely mindig lesz igaz értékű, és ezért minden üzenetet fog lehet kötegelni. Kattintson a **Tovább** gombra.
   
   ![][17]  
4. Adjon meg egy kötegelt kiadási feltételeket. Hello legördülő-lista, válassza ki **MessageCountBased**, és a **száma**, adja meg **3**. Ez azt jelenti, hogy az három üzenetkötegek kapnak tooNorthwind. Kattintson a **Tovább** gombra.
   
   ![][18]  
5. Tekintse át a hello összefoglalása, és kattintson a **mentése**. Kattintson a **telepítés** tooredeploy hello szerződést.
6. Lépjen vissza toohello **oktatóanyag ügyfél**, kattintson a **belső fejlesztésű számla küldése**, hello kér toosend hello számla kövesse. Megfigyelheti, hogy számla nem érkezik a Northwind, mert hello köteg mérete nem teljesül. Ismételje meg ezt a lépést még kétszer tooNorthwind három számla üzenetek, hogy. Ez megfelel a hello kötegelt kiadási feltételeinek, 3 üzenet, és meg kell jelennie a Northwind számla.

<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

