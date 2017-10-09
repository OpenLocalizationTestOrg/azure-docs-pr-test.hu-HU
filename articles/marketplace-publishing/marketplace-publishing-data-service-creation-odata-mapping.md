---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: deb2e52dd03f5beb2ad6a927bd2d03e47d20b691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a>Egy meglévő webes szolgáltatás tooOData keresztül CSDL leképezése
> [!IMPORTANT]
> **Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.** Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners). Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Ez a cikk áttekintést nyújt hogyan toouse egy CSDL toomap egy meglévő service tooan kompatibilis OData-szolgáltatás. Ismerteti, hogyan toocreate hello leképezési dokumentum (CSDL), amely átalakítja a hívások és hello hello ügyfél bemeneti kérése hello kimeneti (adatok) vissza toohello ügyfél kompatibilis OData-adatcsatorna keresztül. Microsoft Azure piactéren szolgáltatások toohello végfelhasználók közzététele hello OData protokoll használatával. Tartalomszolgáltatók (adatok tulajdonosai) által feltárt szolgáltatások érhetők el a többféle formában, mint például a többi, SOAP, stb.

## <a name="what-is-a-csdl-and-its-structure"></a>Mi az a CSDL és struktúrája?
Egy CSDL (Fogalmi séma Definition Language) a meghatározása, hogy toodescribe webszolgáltatás vagy az adatbázis szolgáltatás közös XML szóhasználatára hasonlítanak toohello Azure piactér specifikációval.

Hello – sematikus ábra **kérelem folyamata:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Hello **adatfolyama** hello van ellenkező irányban:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**1. ábra** ábrázolja, hogyan ügyfél volna a szerezhet tartalomszolgáltató (a szolgáltatás) átnézi hello Azure piactéren.  hello CSDL hello leképezési/átalakítása összetevő toohandle hello kérelem használja, és adatokat továbbítani hello tartalomszolgáltató Services-szolgáltatás(ok) és kérő ügyfél hello között.

*1. ábra: Részletes folyamata a kérelmező ügyfélnek toocontent szolgáltató Azure piactéren keresztül*

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Háttér Atom, Atom Pub és hello OData protokoll, mely hello Azure piactér kiterjesztések összeállítása, olvassa el: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Hivatkozás promptjai kivonata: *"hello hello Open Data protokoll (OData alábbiakban említett tooas) célja tooprovide REST-alapú protokoll, CRUD-stílusú műveleteket (létrehozás, Olvasás, frissítés és törlés) adatszolgáltatások elérhető erőforrásokon. A "data"szolgáltatás"" végpont ahol van egy vagy több "gyűjtemények" minden nulla vagy több "bejegyzés", álló származó adatokat adta-e nevű-érték párokat. OData által közzétett Microsoft OASIS (szervezet hello fizetési a strukturált adatokat szabványok) szabványok szerint, hogy bárki toocan szeretne felépítéséhez, kiszolgálók, ügyfelek vagy eszközök jogdíjak és korlátozások nélkül."*

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a>Három kritikus hello CSDL által meghatározott toobe rendelkező van:
* Hello **végpont** a szolgáltató hello hello webes cím (URI-val) hello szolgáltatás
* Hello **Eseményadat-paraméterek** , bemeneti toohello szolgáltató átadta toohello tartalomszolgáltató szolgáltatás le toohello adattípus küldött hello paraméterek hello meghatározását.
* **Séma** hello adatok toohello kérő szolgáltatás hello séma hello tartalomszolgáltató szolgáltatás kézbesíti hello adatok ad vissza, beleértve a tároló, a gyűjtemények, illetve táblákat, a változók/oszlopok és az adatok típus.

a következő diagram hello hello folyamata, ahol hello ügyfél belép hello OData (hívás toohello tartalomszolgáltató webszolgáltatás) utasítás toogetting hello/eredmények vissza az áttekintését mutatja.

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>lépéseket:
1. Ügyfél küld szolgáltatás híváson keresztül kész, de XML toohello Azure piactér definiált bemeneti paraméterek
2. CSDL használt toovalidate hello szolgáltatás hívása.
   * hello szolgáltatás hívása formázott majd toohello tartalom szolgáltatók szolgáltatás által küldött hello Azure piactéren
3. hello Webservice végrehajtja és előforma hello művelettel hello Http-műveletet (pl. GET) hello adat tooAzure piactér, ahol a hello kért adatok (ha van ilyen) tesz elérhetővé, az XML-formátuma toohello ügyfél leképezési definiált hello CSDL hello segítségével.
4. hello ügyfél küldött hello adatokat (ha van ilyen) XML- vagy JSON formátumban

## <a name="definitions"></a>Meghatározások
### <a name="odata-atom-pub"></a>OData ATOM pub
Egy bővítmény toohello ATOM pub, ahol minden bejegyzésben eredményeként a több sorban is állítsa be. hello tartalom hello bejegyzés része továbbfejlesztett toocontain hello értékek hello sor – kulcs-érték párként. További információt itt talál: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - fogalmi séma adatdefiníciós nyelv
Megadhatja a funkciók (SPROCs) és egy adatbázis keresztül közzétett entitásokat. További információt itt talál: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Kattintson a hello **más verzióiban** legördülő lista és a megfelelő verzió kiválasztása, ha nem lát hello cikk.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - bejegyzés adatmodell
* Áttekintés: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Kép: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Adattípusok: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

hello következő mutatja hello részletes ahol hello ügyfél belép hello OData (hívás toohello tartalomszolgáltató webszolgáltatás) utasítás toogetting hello/eredmények vissza a bal oldali tooRight folyamata:

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>CSDL alapjai
Egy CSDL (Fogalmi séma Definition Language) a meghatározása, hogy toodescribe webszolgáltatás vagy az adatbázis szolgáltatás közös XML szóhasználatára hasonlítanak toohello Azure piactér specifikációval. CSDL ismerteti kritikus hello darab, amely **hello adatforrás toohello Azure Piactérről származó adatok áthaladását hello lehetővé teszi.** hello fő eleme a dokumentum ismerteti:

* Az összes nyilvánosan elérhető funkciók (FunctionImport csomópont) leíró adatokat csatoló
* Adattípus-információ az összes üzenet requests(input) és üzenet responses(outputs) (EntityContainer/EntitySet készlet/EntityType csomópontok)
* Kötési információ hello átviteli protokoll toobe használt (fejléc csomópont)
* Címadatok hello keresése a megadott service (a BaseURI attribútum)

Hello CSDL a ez már a vég, hello szolgáltatás kérelmező és hello-szolgáltató platform - és nyelvfüggetlen szerződés jelöli. Hello CSDL, egy ügyfél segítségével keresse meg a webszolgáltatás service, illetve az adatbázis és a nyilvánosan elérhető funkciók meghívni.

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a>Egy CSDL tooa adatbázis vagy egy gyűjtemény
**hello CSDL-specifikációja**

CSDL egy webszolgáltatás-bővítmény leíró XML-nyelvtan. hello specification maga 4 fő elemet oszlik: EntitySet, a következő functionimport elemben; Névtér, és az entitytype típus.

toomake a absztrakciós könnyebb toounderstand lehetővé teszi, hogy egy CSDL tooa táblából.

Ne feledje;

  CSDL jelöli hello platform - és nyelvfüggetlen szerződés **szolgáltatás kérelmező** és hello **szolgáltató**. CSDL, használja a **ügyfél** keresheti meg a **webes szolgáltatás, illetve az adatbázis-szolgáltatás** és a nyilvánosan elérhető meghívni **funkciók.**

Az adatszolgáltatás-hello négy része egy CSDL-re egy adatbázis, táblázatok, oszlopok és eljárás tekintetében.

Vonatkozó ezek egy szolgáltatás számára az alábbiak szerint:

* EntityContainer ~ adatbázis =
* EntitySet ~ = táblázat
* EntityType ~ oszlopok =
* FunctionImport ~ = tárolt eljárás

**HTTP-műveletek engedélyezett**

* GET – a hello adatbázisból (gyűjteményének beolvasása) értéket ad vissza értéket
* POST – használt toopass adatok tooand választható visszatérési értékek a hello adatbázisból (hozzon létre egy új bejegyzést hello gyűjteményben, visszatérési azonosítója/URI-címe)
* TÖRLÉS – hello DB (a gyűjtemény törlése) adatainak törlése
* PUT – adatok frissítéséhez be egy DB (cserélje ki egy gyűjteményt, vagy hozzon létre egyet)

## <a name="metadatamapping-document"></a>Metaadat-hozzárendelés dokumentum
hello metaadat-hozzárendelés dokumentum a tartalomszolgáltató meglévő webszolgáltatásokat, hogy akkor is elérhető egy OData webszolgáltatást hello Azure piactér rendszer által használt toomap. CSDL alapul, és megvalósítja néhány bővítmények tooCSDL tooaccommodate hello igényeinek REST alapú webszolgáltatások közzétéve az Azure piactéren. hello bővítmények találhatók hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) névtér.

A következő egy példa hello CSDL: (Másolás és beillesztés hello az alábbi példában CSDL az XML-szerkesztő, és módosítsa a toomatch a szolgáltatáshoz.  Majd illessze be CSDL-leképezési DataService lapon a való létrehozásakor a szolgáltatás hello [Azure piactér közzétételi Portáljára](https://publish.windowsazure.com)).

**Feltételek:** kapcsolatos hello CSDL kifejezések toohello [közzétételi Portáljára](https://publish.windowsazure.com) felhasználói felület (PPUI) feltételeit.

* Ajánlat hello PPUI "Title" tooMyWebOffer konfigurációelemmel kapcsolatos
* Hello PPUI az értéket túl vonatkozik**Publisher megjelenített név** a hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) felhasználói felület
* Az API-vonatkozik tooa Web vagy adatszolgáltatás (egy csomag hello PPUI)

**Hierarchia:** egy vállalat (tartalomszolgáltató) tulajdonában lévő esetében, amelyek Plan(s), nevezetesen service(s), mely sor fel egy API-t.

### <a name="webservice-csdl-example"></a>Webszolgáltatás CSDL-példa
Tooa szolgáltatás, amely a webes alkalmazás a végpont (például egy C#-alkalmazás) adategyezményt alkalmazza csatlakozik

        <?xml version="1.0" encoding="utf-8"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all hello web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition near hello bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is hello entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines hello service call, replace & with hello encode value (&amp;).
        In hello input name value pairs {param} represents passed in value.
        Or hello value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows hello CSDL toospecifically specify hello verb of hello service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes hello parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how hello web service call is exposed toomarketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, hello {...} point toohello parameters defined below.
        Marketplace will read hello parameters from this URITemplate and fill hello values into hello corresponding parameters of hello BaseUri or RequestBody (below) during calls tooyour service.  
        It is okay for @d:BaseUri tooinclude information only for Marketplace consumption, it will not be exposed tooend users. i.e. hello hardcoded AccountKey in hello above BaseURI does not show up in hello client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of hello RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders tooinsert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how toopass userid and password via hello header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed tooend users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with hello value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited tooappearing as hello value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used toospecify values tooinsert into hello ATOM feed returned toohello end user.  You can specify hello element toocontain a fixed message by providing a value for hello element (this is hello default value).  @d:Map is an XPath expression that points into hello response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay tooalso add a d:Map attribute toooverride this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of hello web service call:
        @Name should match exactly (case sensitive) hello {…} placeholders in hello @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether hello parameter is required.
        @d:Regex - optional, attribute toodescribe hello string, limiting unwanted input at hello entry of hello system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in hello Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating hello service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in hello order listed.
        @d:Match is an Xpath query on hello service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is hello error code tooreturn if an response matches hello error.
        @d:errorMessage is hello user friendly message tooreturn when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect toohello service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- hello EntityContainer defines hello output data schema -->
        </EntityContainer>
        <!-- hello EntityType @d:Map defines hello repeating node (an XPath query) in hello response (output data schema). -->
        <!-- If these nodes are outside a namespace, add hello prefix in hello xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in hello ATOM feed, so comply with hello XML element naming restrictions (no spaces or other illegal characters).
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query toopoint at hello location tooextract hello content from your services response.
        hello "." is relative toohello repeating node in hello EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> További CSDL-webszolgáltatás példákat cikkében hello cikk [példák leképezése egy meglévő webes szolgáltatás tooOData CSDLs keresztül](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>DataService CSDL-példa
Csatlakozás tooa szolgáltatás, amely a adategyezményt alkalmazza adatbázis tábla vagy nézet végpont az alábbi példában látható két API-khoz, az adatbázis-alapú API CSDL (használhat nézetek táblák helyett).

        <?xml version="1.0"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all hello data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of hello EntitySet as a Service
        @Name is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); hello table (or view) and columns toobe returned by hello data service.)
            Map is hello schema.tabel or schema.view
            dals.TableName is hello table Name
            Name is hello name identifier for hello EntityType and hello Name of hello service exposed toohello client via hello UI.
            dals:IsExposed determines if hello table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is hello schema name of hello table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines hello column properties and hello output of hello service.
            dals:ColumnName is hello name of hello column in hello table /view.
            Type is hello emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is hello maximum length of hello output value
            Name is hello name of hello Property and is exposed toohello client facing UI
            dals:IsReturned is hello Boolean that determines if hello Service exposes this value toohello client.
            IsQueryable is hello Boolean that determines if hello column can be used in a database query
            (For data Services: tooimprove Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is hello numerical position x in hello table or hello View, where x is from 1 toohello number of columns in hello table.
            dals:DatabaseDataType is hello data type of hello column in hello database, i.e. SQL data type dals:IsPrimaryKey indicates if hello column is hello Primary key in hello table/view.  (hello columns marked ISPrimaryKey are used in hello Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Lásd még:
* Ha érdekli tanulási és ismertetése hello adott csomópontok és a paraméterek, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.
* Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee példakód és kód szintaxis és a környezetben.
* Ebben a cikkben egy adatszolgáltatás toohello Azure piactér-közzététel elérési előírt tooreturn toohello [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).

