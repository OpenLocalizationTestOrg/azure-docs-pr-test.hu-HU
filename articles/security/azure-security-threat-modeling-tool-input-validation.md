---
title: "-Microsoft fenyegetések modellezése eszköz - érvényesítési Azure aaaInput |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 823503881f4bae292ef021834d5e64acf2a0f54a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-input-validation--mitigations"></a>Biztonsági keret: Bemeneti ellenőrzés |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Webalkalmazás** | <ul><li>[A nem megbízható stíluslapok használó összes átalakítások scripting XSLT letiltása](#disable-xslt)</li><li>[Győződjön meg arról, hogy minden felhasználó vezérelhető tartalmat sikerült tartalmazó oldalon jelentésküldési kívül automatikus MIME-elemzése](#out-sniffing)</li><li>[Alakuljanak vagy XML entitás tiltása](#xml-resolution)</li><li>[Alkalmazások használata a http.sys hajtsa végre az URL-cím kanonizálási ellenőrzése](#app-verification)</li><li>[Győződjön meg arról, megfelelő vezérlők érvényben vannak, amikor a felhasználók fájlokat elfogadása](#controls-users)</li><li>[Győződjön meg arról, hogy biztonságos típusú paraméterek használ a webalkalmazás adatelérési](#typesafe)</li><li>[Külön modell kötés osztály használatát, vagy kötési listáit tooprevent MVC tömeges hozzárendelés biztonsági rés](#binding-mvc)</li><li>[Nem megbízható webes kimeneti előzetes toorendering kódolása](#rendering)</li><li>[Hajtsa végre a bemenet-ellenőrzéshez és a szűrést az összes karakterlánc típusú modell tulajdonságai](#typemodel)</li><li>[Tisztítási mezőknek, amelyekben az összes karakterek, például azt, rich text editor alkalmazandó](#richtext)</li><li>[Ne rendeljen DOM elemek toosinks, amely nem rendelkezik beépített kódolás](#inbuilt-encode)</li><li>[Az összes ellenőrzése hello alkalmazáson belül átirányítások lezárt vagy biztonságosan történik](#redirect-safe)</li><li>[Megvalósítása bemenet-ellenőrzést az összes karakterlánc típusú paramétert fogadja el a tartományvezérlő módszerek](#string-method)</li><li>[A reguláris kifejezés feldolgozása miatt toobad reguláris kifejezések tooprevent DoS felső korlátja időtúllépés beállítása](#dos-expression)</li><li>[Kerülje a Html.Raw Razor nézetekben](#html-razor)</li></ul> | 
| **Adatbázis** | <ul><li>[Ne használjon dinamikus lekérdezések a tárolt eljárások](#stored-proc)</li></ul> |
| **Webes API** | <ul><li>[Arról, hogy Modellellenőrzés a webes API-módszer](#validation-api)</li><li>[Megvalósítása bemenet-ellenőrzést az összes karakterlánc típusú paraméterek fogadja el a webes API-módszer](#string-api)</li><li>[Ellenőrizze, hogy biztonságos típusú paraméterek szolgáltatás webes API-ban az adatelérési](#typesafe-api)</li></ul> | 
| **Az Azure Document DB rendszerbe** | <ul><li>[A DocumentDB paraméteres SQL-lekérdezések használata](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[WCF bemeneti érvényesítési séma kötését keresztül](#schema-binding)</li><li>[WCF - bemenet érvényesítési paraméter ellenőrök keresztül](#parameters)</li></ul> |

## <a id="disable-xslt"></a>A nem megbízható stíluslapok használó összes átalakítások scripting XSLT letiltása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [XSLT biztonsági](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript tulajdonsággal](http://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Lépések** | XSLT támogatja belül hello segítségével stíluslapok scripting `<msxml:script>` elemet. Ez lehetővé teszi az egyéni függvényekhez toobe XSLT-átalakítás szerepel. hello parancsfájl végrehajtása hello folyamat végrehajtása hello átalakító hello környezetében. XSLT-parancsfájl egy nem megbízható kód nem megbízható környezet tooprevent végrehajtását, amely le kell tiltani. *Ha a .NET használatával:* XSLT-parancsfájlok alapértelmezés szerint le van tiltva; azonban győződjön meg arról, hogy nincs explicit módon Engedélyezés keresztül hello `XsltSettings.EnableScript` tulajdonság.|

### <a name="example"></a>Példa 

```C#
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Példa
Ha használja az MSXML 6.0 használatával, XSLT-scripting alapértelmezésben le van tiltva; azonban úgy kell beállítania, hogy nem explicit módon Engedélyezés hello XML DOM objektumtulajdonság AllowXsltScript keresztül. 

```C#
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Példa
Ha az MSXML 5 használ, vagy az alábbi XSLT-parancsfájlok alapértelmezés szerint engedélyezve van, és explicit módon kell tiltsa le azt. Hello XML DOM objektum tulajdonság AllowXsltScript toofalse beállítása. 

```C#
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting toofalse disables XSLT scripting.
```

## <a id="out-sniffing"></a>Győződjön meg arról, hogy minden felhasználó vezérelhető tartalmat sikerült tartalmazó oldalon jelentésküldési kívül automatikus MIME-elemzése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [IE8 Biztonsági rész - átfogó védelmének](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Lépések** | <p>Az egyes lapok tartalmazhatnak felhasználói ellenőrizhető tartalmat, használnia kell a HTTP-fejléc hello `X-Content-Type-Options:nosniff`. toocomply a követelmény abból, vagy set hello szükséges fejléc oldalanként csak a felhasználó vezérelhető tartalmat tartalmazó lapok is, vagy beállíthatja azt globálisan hello alkalmazás összes lapja esetében.</p><p>A webkiszolgáló-i fájltípusok tartozik egy [MIME-típus](http://en.wikipedia.org/wiki/Mime_type) (más néven egy *tartalomtípus*), amely leírja, hogy hello jellege hello tartalom (Ez azt jelenti, kép, szöveg, alkalmazás, stb.)</p><p>X-tartalom-típust-beállítások hello fejléc egy HTTP-fejlécet, amely lehetővé teszi a fejlesztők számára, hogy a tartalmat nem kell MIME-felszippantásra toospecify. Ezt a fejlécet tervezett toomitigate MIME-elemzése támadások. Ezt a fejlécet támogatása az Internet Explorer 8 (IE8) lett hozzáadva.</p><p>Csak az Internet Explorer 8 (IE8) felhasználók élvezi, az X-tartalom-típust-beállítások. Az Internet Explorer korábbi verzióiban jelenleg nem veszik figyelembe hello X-tartalom-típust-beállítások fejléc</p><p>Az Internet Explorer 8 (és újabb) rendszer hello csak ismertebb böngésző MIME-elemzése tooimplement lemondáshoz szolgáltatás. Ha más ismertebb böngésző (Firefox, Safari, Chrome) valósít meg hasonló funkciókat, ez a javaslat kell-e a frissített tooinclude szintaxisát, valamint ezek támogatják</p>|

### <a name="example"></a>Példa
tooenable hello szükséges fejléc globálisan hello alkalmazás összes lapja esetében, tegye hello következők egyikét: 

* Hello fejléc hozzáadása hello web.config fájlban, ha hello alkalmazás által az Internet Information Services (IIS) 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Hello keresztül hello fejléc hozzáadása globális alkalmazás\_BeginRequest 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Egyéni HTTP-modulja megvalósítása 

``` 
public class XContentTypeOptionsModule : IHttpModule 
  {
    #region IHttpModule Members 
    public void Dispose() 
    { 

    } 
    public void Init(HttpApplication context)
    { 
      context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders); 
    } 
    #endregion 
    void context_PreSendRequestHeaders(object sender, EventArgs e) 
      { 
        HttpApplication application = sender as HttpApplication; 
        if (application == null) 
          return; 
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* Hello szükséges fejléc csak az adott lapok tooindividual válaszok hozzáadással engedélyezheti: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Alakuljanak vagy XML entitás tiltása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [XML entitás bővítése](http://capec.mitre.org/data/definitions/197.html), [XML szolgáltatásmegtagadási támadások és Védelmekkel](http://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML biztonsági áttekintése](http://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [gyakorlati tanácsok biztonságossá tétele az MSXML kód](http://msdn.microsoft.com/library/ms759188(VS.85).aspx), [ NSXMLParserDelegate protokoll referenciája](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [külső hivatkozások feloldása](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Lépések**| <p>Bár nem elterjedt, nem az XML-kód, amely lehetővé teszi, hogy hello XML parser tooexpand makró entitások hello dokumentum maga belül vagy külső forrásból definiált értékekkel. Meghatározhatja például, hello dokumentum előfordulhat, hogy egy entitás "#companyname" hello "Microsoft" értékű, hogy minden egyes hello szöveg "&companyname;" hello dokumentumban jelenik meg automatikusan a Microsoft hello szöveg helyére. Vagy hello dokumentum definiálhat egy entitás "MSFTStock", amely egy külső webes szolgáltatás toofetch hello aktuális Microsoft készlet értéke hivatkozik.</p><p>Bármikor majd "&MSFTStock;" hello dokumentumban jelenik meg automatikusan hello aktuális árfolyam helyére. Ez a funkció azonban nem visszaélt toocreate szolgáltatásmegtagadásos (DoS) szolgáltatási feltételek. Egy támadó ágyazhatja több entitások toocreate egy exponenciális bővítése XML bomba, amely akkor hello rendszer összes rendelkezésre álló memória. </p><p>Azt is megteheti hogy hozhat létre egy külső hivatkozást, amely az adatfolyamokat, vissza végtelen adatmennyiséget, vagy egyszerűen lefagy hello szál. Ennek eredményeképpen az összes csoport le kell tiltania belső vagy külső XML entitás feloldási teljesen, ha az alkalmazás nem használja, vagy manuálisan is korlátozható hello memóriamennyiség és hello alkalmazás entitás a névfeloldás vehet igénybe, ha ez a funkció idő feltétlenül szükséges. Ha az alkalmazás nem követeli meg entitás megoldás, majd tiltsa le azt. </p>|

### <a name="example"></a>Példa
.NET-keretrendszer kódot használhatja a következő módszerekkel hello:

```C#
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
Vegye figyelembe, hogy hello alapértelmezett értékének `ProhibitDtd` a `XmlReaderSettings` értéke igaz, de a `XmlTextReader` azt értéke false. Ha használ XmlReaderSettings elemben, tooset ProhibitDtd tootrue nem kell explicit módon, de ajánlott biztonsági szakét, hogy végezzen. Ne feledje, hogy engedélyezi-e hello XmlDocument osztály entitás feloldási alapértelmezés szerint. 

### <a name="example"></a>Példa
toodisable entitás felbontása a XmlDocuments, használjon hello `XmlDocument.Load(XmlReader)` hello túlterhelése betöltésére metódust és hello megfelelő tulajdonságainak beállítása hello XmlReader argumentum toodisable közben a következő kód hello ismertetett módon: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Példa
Ha a letiltása entitás feloldása nem lehetséges, hogy az alkalmazás, hello XmlReaderSettings.MaxCharactersFromEntities tulajdonság tooa ésszerű értékének beállítása tooyour alkalmazás igényeinek megfelelően. Ez korlátozza hello lehetséges exponenciális bővítése szolgáltatásmegtagadási támadások káros hatása. a következő kód hello ezt a módszert mutatja: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Példa
Ha tooresolve beágyazott entitásokat szükséges, de nem kell tooresolve külső entitások, állítsa be a hello XmlReaderSettings.XmlResolver tulajdonság toonull. Példa: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Vegye figyelembe, hogy az MSXML6, ProhibitDTD tootrue (a DTD feldolgozásának letiltása) alapértelmezés szerint van beállítva. Apple OSX/iOS kód nincsenek használható két XML elemzők: NSXMLParser és libXML2. 

## <a id="app-verification"></a>Alkalmazások használata a http.sys hajtsa végre az URL-cím kanonizálási ellenőrzése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>A http.sys használó összes alkalmazásra kell kövesse az alábbi irányelveket:</p><ul><li>Korlátozza a hello URL-cím hossza toono több mint 16 384 karakterek (ASCII vagy Unicode). Ez a hello abszolút URL-cím hosszabb hello alapértelmezett Internet Information Services (IIS) 6 beállítás alapján. Webhelyek törekedni kell egy rövidebb, mint ez a lehetőség szerint</li><li>Ezek fogják kihasználni a hello kanonizálási szabályok a hello .NET FX hello szabványos .NET-keretrendszer fájl i/o-osztály (például a FileStream) használja</li><li>Explicit módon az egy ismert fájlnevek engedélyezési-lista létrehozása</li><li>Kifejezetten elutasítja az ismert fájl típusai nem teljesíti a UrlScan elutasítások: exe, bat, cmd, com, IDA, ida, idq, htr, idc, shtm [l], stm, nyomtató, ini, pol, dat fájlok</li><li>A következő kivételek hello catch:<ul><li>System.ArgumentException (az eszköz neve)</li><li>(Az adatfolyamokat) System.NotSupportedException</li><li>System.IO.FileNotFoundException (a érvénytelen escape-karakterrel megjelölt fájlnév)</li><li>(A érvénytelen escape-karakterrel megjelölt könyvtárak) System.IO.DirectoryNotFoundException</li></ul></li><li>*Ne* tooWin32 fájl i/o-API hívásához. Az egy URL-cím érvénytelen szabályosan térjen vissza a 400 hiba toohello felhasználó, és hello valós hiba jelentkezik.</li></ul>|

## <a id="controls-users"></a>Győződjön meg arról, megfelelő vezérlők érvényben vannak, amikor a felhasználók fájlokat elfogadása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Fájl feltöltése korlátozás](https://www.owasp.org/index.php/Unrestricted_File_Upload), [aláírás tábla](http://www.garykessler.net/library/file_sigs.html) |
| **Lépések** | <p>Feltöltött fájlok egy jelentős kockázat tooapplications képviseli.</p><p>számos támadás hello első lépéseként néhány kódot toohello rendszer toobe megtámadott tooget. Hello támadás majd csak egy módon tooget hello kód végrehajtása toofind van szüksége. A fájlfeltöltés segítségével hello támadó hello első lépés elvégzését. korlátlan fájlfeltöltés hello következményeit változhat, beleértve a teljes rendszer felvásárlási, egy túlterhelt fájlrendszer vagy adatbázis támadások tooback-a befejezési rendszerek, és egyszerű felülírása továbbítása.</p><p>Ez függ, milyen hello alkalmazás nem hello feltöltött fájl, és különösen helyén. Kiszolgáló-oldali ellenőrzés fájlfeltöltéseket hiányzik. A következő biztonsági vezérlők fájlfeltöltés funkciójának kell végrehajtani:</p><ul><li>Fájl kiterjesztése ellenőrzése (csak engedélyezett fájltípus érvényes meghatározott fogadhatók el)</li><li>Maximális méretkorlátot.</li><li>A fájl nem lehet feltöltött toowebroot; hello helyre rendszermeghajtó nem egy könyvtár kell lennie</li><li>Az elnevezési konvenciót kell követni, úgy, hogy hello feltöltött fájl neve rendelkezik néhány annak véletlenszerű, így mint tooprevent fájl felülírja</li><li>Fájlok kell futtatni víruskereső toohello lemez írása előtt</li><li>Hello fájl nevét és minden egyéb metaadatot (például a fájl elérési útja) rosszindulatú karakterek érvényesítése érdekében meg</li><li>Fájlaláírás formátumú kell ellenőrizni, tooprevent masqueraded fájlt feltölteni a felhasználó (pl. feltöltése egy exe-fájl kiterjesztése tootxt módosításával)</li></ul>| 

### <a name="example"></a>Példa
Hello utolsó pont fájl formátumának aláírás érvényesítése kapcsolatban tekintse meg a toohello osztály alábbi részleteket: 

```C#
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a id="typesafe"></a>Győződjön meg arról, hogy biztonságos típusú paraméterek használ a webalkalmazás adatelérési

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Hello paramétergyűjtemény használatakor SQL kezeli hello bemeneti van, mint végrehajtható kód, nem konstans érték. hello paramétergyűjtemény lehet használt tooenforce típusa és hossza korlátozza a bemeneti adatok. Hello tartományon kívül eső értékeket kivételt vált. Típus környezetben is biztonságos SQL-paraméterek nem használják, ha a támadók lehet szűrés hello bemeneti beágyazott képes tooexecute injektálási támadások.</p><p>Felhasználhatja a biztonságos Típusparaméterek tooavoid lehetséges SQL injektálási támadásokkal szemben, amelyek alakulhat ki nem szűrt bemeneti hozhat létre, SQL lekérdezések. Biztonságos Típusparaméterek is használhatja, tárolt eljárások és a dinamikus SQL-utasításokat. Paraméterek literálértékek hello adatbázis által, nem pedig végrehajtható kód kell kezelni. Paraméterek típusa és hossza is ellenőrzi.</p>|

### <a name="example"></a>Példa 
hello következő kód bemutatja, hogyan toouse adja-e biztonságos paramétereket hello SqlParameterCollection tárolt eljárás meghívásakor. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Az előző példakódban hello hello bemeneti érték nem lehet hosszabb, mint 11 karakter. Ha hello adat nem felel meg a toohello típus vagy hello paraméter által meghatározott, hello SqlParameter osztály kivételt jelez. 

## <a id="binding-mvc"></a>Külön modell kötés osztály használatát, vagy kötési listáit tooprevent MVC tömeges hozzárendelés biztonsági rés

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Metaadat-attribútumok](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [nyilvános kulcs biztonsági biztonsági rés és a megoldás](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [teljes körű útmutatót tooMass ASP.NET MVC-hozzárendelést](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [MVC használatával EF első lépések](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Lépések** | <ul><li>**Mikor kell keresni túlküldéses biztonsági rések? -** Túlküldéses biztonsági rések modell osztályok kötést létrehozni a felhasználói bevitel bárhol fordulhat. Például MVC keretrendszerek jelenthet a felhasználói adatok .NET egyéni osztályokat, beleértve az egyszerű régi CLR objektumok (POCOs). MVC automatikusan feltölti a ezen modell osztályok hello kérelemből adatokkal biztosít a felhasználói bevitel kapcsolatos kényelmes megjelenítése. Ha ezeket az osztályokat, amely nem állítható hello felhasználó tulajdonságai, hello alkalmazás lehet sebezhető tooover feladási támadásokat, amelyek lehetővé teszik a felhasználói vezérlő által hello alkalmazás soha nem javasolt. MVC modellkötést, például adatbázis hozzáférési technológiák, például a objektum relációs mappers Entity Framework például gyakran is támogatja a POCO objektumok toorepresent adatbázis adatokkal. Ezen adatok modell osztályok hello adja meg az adatbázis adatok MVC nem felhasználói bevitel kezelésével foglalkozó azonos kényelmi célokat szolgál. Mivel MVC és az hello adatbázis támogatja hasonló modellek POCO objektumok, például azonos mindkét célokra osztályok egyszerűen tooreuse hello tűnik. Az eljárás sikertelen toopreserve elkülönítése vonatkozik, és egy közös területen, ahol nem kívánt tulajdonságok kitett toomodel kötés túlzott könyvelési támadások engedélyezése.</li><li>**Miért ne használja paraméterek toomy MVC műveletként a modell nem szűrt adatbázisosztályokkal? -** Mert MVC modellkötést köti, semmit a szóban forgó osztályban. Akkor is, ha hello adatok nem jelennek meg a nézet egy rosszindulatú felhasználó képes HTTP-kérelem elküldeni ezeket az adatokat tartalmazza, és az MVC legszívesebben köti, mert a művelet szerint a adatbázis osztály hello alakzat adatok hogy fogadja el a felhasználói bevitelhez.</li><li>**Miért kell modell kötések létrehozására használt hello alakzat érdeklik? -** Modellkötést használ az ASP.NET MVC túlságosan széleskörű modellekkel való közzétesz egy alkalmazást tooover feladási támadások. Túlzott könyvelési lehetővé teszi a támadók toochange alkalmazás adatok milyen hello fejlesztőknek szánt, például egy cikk vagy hello biztonsági jogosultságok egy olyan fiók hello ára felülbírálása. Alkalmazások művelet-specifikus modellek (vagy adott engedélyezett szűrő tulajdonságlisták) kötés tooprovide egy explicit szerződés milyen nem megbízható bemeneti tooallow modellkötést keresztül kell használnia.</li><li>**Csak a kód duplikálását külön kötést modellek nehézségekkel? -** Nem, akkor aggályokat elkülönítését pár. Ha műveletmetódusokhoz adatbázis modellek szeretné újrafelhasználni, akkor véleményét bármely (vagy az alárendelt tulajdonság) abban a osztály hello felhasználó HTTP-kérelem által állítható be. Ha ez nem ajánlott MVC toodo van szüksége egy szűrő, vagy egy külön osztály alakzat tooshow MVC milyen adatok származhatnak felhasználói bevitel helyette.</li><li>**Ha külön kötést modellek a felhasználói bevitelhez, kell tooduplicate saját adatok jegyzet attribútumok? -** Nem feltétlenül. Hello adatbázis modell osztály toolink toohello metaadatok kötés modellosztállyal MetadataTypeAttribute használhatja. Csak olyan vegye figyelembe, hogy a hello hello MetadataTypeAttribute által hivatkozott típus (rendelkezhet kevesebb tulajdonság, de nem lehet több) típusú hivatkozó hello részhalmazát kell lennie.</li><li>**Adatok áthelyezése oda-vissza felhasználói bemeneti modellek és adatbázis-modellek között fárasztó. Is szeretnék csak másolja át az összes tulajdonság a következőt reflexió használatával? -** Igen. hello csak hello kötés modellek megjelenő tulajdonságok megadta, hogy felhasználói bevitel biztonságosként toobe hello néhányat a meglévők közül. Nincs, amely megakadályozza a reflexió toocopy összes tulajdonságai között két modell közös létező protokollt használó biztonsági OK.</li><li>**Mi a helyzet [kötése (kizárása = "â" ¦")]. Használható, amelyek ahelyett, hogy külön kötést modellek? -** Ez a módszer nem ajánlott. Használatával [kötése (kizárása = "â" ¦")] azt jelenti, hogy minden új tulajdonság köthető alapértelmezés szerint. Amikor új tulajdonság ad hozzá, egy további lépést tooremember tookeep dolog van biztonságos, nem pedig hello tervezési képezniük alapértelmezés szerint biztonságos. Attól függően, hogy hello fejlesztői ellenőrzése a lista minden alkalommal, amikor egy tulajdonság hozzáadása kockázatos.</li><li>**Van [kötése (Belefoglalás = "â" ¦")] Szerkesztés műveletekhez hasznos? -** Nem. [Kötése (Belefoglalás = "â" ¦")] csak alkalmas INSERT stílusú operations (az új adatok hozzáadása). A frissítés stílusú műveleteket (a meglévő adatok módosítása), egy másik módszert használja, például külön kötést modellek rendelkezik, vagy átadja tulajdonságok engedélyezett tooUpdateModel vagy TryUpdateModel explicit listáját. Hozzáadása egy [kötése (Belefoglalás = ":" ¦")] attribútum szerkesztési művelet azt jelenti, hogy MVC fog egy objektumpéldányra csak hello felsorolt tulajdonságai, minden más alapértelmezett értékükön hagyja beállítású. Amikor hello adatok, azzal lecseréli hello meglévő entitás hello bármely elhagyott tulajdonságok tootheir alapértelmezett értékek visszaállítása teljesen. Például, ha IsAdmin kimaradt egy [kötése (Belefoglalás = ":" ¦")] attribútum szerkesztési művelet esetén minden olyan felhasználó, amelynek a neve szerkesztették keresztül ez a művelet alaphelyzetbe állítása tooIsAdmin lenne = false (bármely módosított felhasználói elveszítik rendszergazdai állapota). Ha azt szeretné, hogy tooprevent toocertain tulajdonságok frissíti, valamelyikével hello más megoldások fent. Vegye figyelembe, hogy egyes verziói MVC tooling készítése tartalmazó vezérlő osztályok [kötése (Belefoglalás = ":" ¦")] műveletek szerkesztése és utalnak, hogy a lista alapján tulajdonság eltávolítása megakadályozza a túlzott könyvelési támadásokat. Azonban a fent leírtaknak megfelelően ezt a megközelítést nem működik megfelelően tartalmaz, és helyette alaphelyzetbe egyetlen megadott adattal sem hello nincs megadva tulajdonságok tootheir alapértelmezett értékeket.</li><li>**A Create műveleteket, vannak-e bármilyen figyelmeztetések segítségével [kötése (Belefoglalás = "â" ¦")] ahelyett, hogy külön kötést modellek? -** Igen. Ez a megközelítés először nem működik Szerkesztés forgatókönyvek esetén igénylő összes túlzott könyvelési biztonsági rés kiküszöböléséhez két külön megközelítés karbantartása. Második, különálló kötés modellek válassza szét az adatmegőrzési, használt felhasználói bevitel és hello alakzat hello alakzatot közötti kérdések valami [kötése (Belefoglalás = "â" ¦")] nem teszi meg. Harmadik, vegye figyelembe, hogy [kötése (Belefoglalás = "â" ¦")] kezelni tud a csak legfelső szintű tulajdonságok; az alárendelt tulajdonságait (például "Details.Name") csak bizonyos részei nem engedélyezheti a hello attribútumban. Végül, és lehet, hogy a legfontosabb, használatával [kötési (Include = "â" ¦")] ad hozzá egy további lépést, amely minden alkalommal hello osztály a modellkötést használható veszni. Ha egy új műveletmetódus toohello adatok osztály közvetlenül van kötve, és tooinclude elfelejti egy [kötési (Belefoglalás = ":" ¦")] attribútum, azok sebezhető tooover feladási támadások, így hello [kötése (Belefoglalás =": "¦")] Alapértelmezés szerint kevésbé biztonságos megoldás. Használatakor [kötése (Belefoglalás = "â" ¦")], gondoskodunk mindig tooremember toospecify azt minden alkalommal, amikor a adatosztályokat művelet a metódusok paramétereihez jelennek meg.</li><li>**A létrehozás műveleteket, mi a helyzet üzembe hello [kötése (Belefoglalás = "â" ¦")] hello modell osztály attribútuma? Elkerülése érdekében hello kell tooremember üzembe hello attribútumot minden tartozó műveleti módszer nem ez a megközelítés? -** Ezt a módszert használja bizonyos esetekben működik. Használatával [kötése (Include = "â" ¦")] hello modell típus saját magát (és nem a művelet paramétereinek Ez az osztály használatával), elkerülheti a hello kell tooremember tooinclude hello [kötési (Belefoglalás ="â "¦")] attribútumot minden tartozó műveleti módszer. Hello attribútum közvetlenül a hello osztály elkülöníti egy külön felületének Ez az osztály a modell adatkötéshez. Azonban ez a megközelítés csak egy modell kötés alakzat / modellosztállyal lehetővé teszi. Ha egy tartozó műveleti módszer kell tooallow modellkötést egy mező (például egy csak rendszergazdai műveletet, amely frissíti a felhasználói szerepkörök), és egyéb műveleteket kell ennek a mezőnek tooprevent modellkötést, ez a módszer nem fog működni. Minden osztály csak rendelkezhet egy modell kötés alakzat; Ha különböző műveleteket kell másik modellhez kötés alakzatok, ezeket külön alakzatok vagy külön modell kötés osztályokat, vagy külön toorepresent szükséges [kötése (Belefoglalás = "â" ¦")] hello műveletmetódusokhoz attribútumok.</li><li>**Mi köti modellek? , Azokat hello ugyanaz, mint a modellek? -** Ezek a két kapcsolódó fogalmak. hello kötés modell hivatkozik tooa modellosztállyal művelet használt kifejezés paraméterlista (MVC modell kötés toohello művelet metódusból átadott hello alakzat). hello kifejezés nézet modell tooa modellosztállyal kapott egy művelet metódus tooa nézet hivatkozik. A nézet jellemző modellek használata egy általánosan használt megközelítés beállítását adatok átadására művelet metódus tooa nézetből. Gyakran ez az alakzat is modellkötést alkalmas, és hello kifejezés nézet modell lehet használt toorefer hello ugyanannak a modellnek mindkét helyen használja. pontos toobe, ezzel az eljárással beszél kifejezetten modellek toohello művelet, amely fontos információk tömeges hozzárendelés céljából átadott hello alakzat összpontosító kötés.</li></ul>| 

## <a id="rendering"></a>Nem megbízható webes kimeneti előzetes toorendering kódolása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, Web Forms, MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hogyan tooprevent többhelyes scripting lehetőséget az ASP.NET](http://msdn.microsoft.com/library/ms998274.aspx), [parancsfájlok](http://cwe.mitre.org/data/definitions/79.html), [lehetővé (helyközi telephelyek közötti) megelőzési Cheat lap](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Lépések** | Az online szolgáltatások vagy a bármely alkalmazás/összetevő, amely akkor hello webes bemenetének felhasználók támadási vektorainak többhelyes parancsfájl-kezelési (általában lehetővé rövidítése). Lehetővé biztonsági rések előfordulhat, hogy egy támadó tooexecute parancsfájl a másik felhasználó sebezhető webes alkalmazás segítségével. Rosszindulatú használt toosteal cookie-kat és a parancsfájlok egyébként illetéktelenül módosítani a áldozat géppel JavaScript keresztül. Felhasználói bevitel ellenőrzése, győződjön meg arról, hogy helyes formátumú-e, és egy weblapon megjelenítése előtt kódolás lehetővé miatt. Bemenet-ellenőrzéshez és a kimeneti kódolás webes védelmi könyvtár használatával elvégezhető. A felügyelt kód (C\#, VB.net stb.), használja egy vagy több megfelelő hello webes védelmi (kártevőirtó-lehetővé) könyvtár, attól függően, hogy hello környezetben, ahol hello felhasználói bevitel számos lekérdezi a kódolási módszert:| 

### <a name="example"></a>Példa

```C#
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a id="typemodel"></a>Hajtsa végre a bemenet-ellenőrzéshez és a szűrést az összes karakterlánc típusú modell tulajdonságai

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Érvényesítési hozzáadása](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [modell az adatok egy MVC alkalmazás](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [irányadó elvek az ASP.NET MVC-alkalmazások](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Lépések** | <p>Minden hello bemeneti paraméterek ellenőrizni kell, mielőtt a használatuk hello alkalmazás tooensure, hogy rosszindulatú felhasználói bevitelek elleni hello alkalmazás biztosítják. Hello bemeneti értékek reguláris kifejezés érvényesítést kiszolgáló oldalán egy engedélyezési lista érvényesítési stratégiával ellenőrizni. Felhasználói bevitel unsanitized / toohello módszerek átadott paraméterek okozhat kód injektálási biztonsági réseket.</p><p>A webes alkalmazásokhoz, a belépési pontok is tartalmazhatnak, űrlapmezők, a QueryStrings, a cookie-k, a HTTP-fejlécek és a webszolgáltatás-paramétereket.</p><p>hello következő bemenet-ellenőrzéshez ellenőrzést kell végrehajtani modellkötést után:</p><ul><li>hello modell tulajdonságai kell feliratozni a reguláris kifejezés megjegyzés, a megengedett karakterek és a maximális megengedett hossz</li><li>hello vezérlő metódusokhoz végre kell hajtania ModelState érvényességi</li></ul>|

## <a id="richtext"></a>Tisztítási mezőknek, amelyekben az összes karakterek, például azt, rich text editor alkalmazandó

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Nem biztonságos bemenet kódolása](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML Sanitizer](https://github.com/mganss/HtmlSanitizer) |
| **Lépések** | <p>Az összes statikus jelölő címkék, amelyet az toouse azonosítása. A gyakori eljárás toosafe HTML elemek, például a formázás toorestrict `<b>` (félkövér) és `<i>` (dőlt).</p><p>Hello adatok HTML-kódolja írása előtt azt. A rosszindulatú parancsfájlok futtatásra toobe okozza kezelt szövegként, nem pedig a végrehajtható kód elérhetővé válnak.</p><ol><li>Tiltsa le az ASP.NET-kérelmek hello hozzáadásával érvényesítési hello ValidateRequest = "false" attribútum toohello lap direktívájában @</li><li>Hello HtmlEncode módszerrel hello karakterlánc bemenet kódolása</li><li>A StringBuilder és a név felülírandó metódus tooselectively eltávolítása kódolás hello HTML-elem, amelyet az toopermit hello hívás</li></ol><p>hello lap a hello hivatkozások letiltja a kérelem ellenőrzése ASP.NET úgy, hogy `ValidateRequest="false"`. Az HTML-kódolja hello adja meg, és szelektív módon lehetővé teszi, hogy hello `<b>` és `<i>` azt is megteheti, a .NET könyvtár az HTML tisztítási is használható.</p><p>HtmlSanitizer a .NET-szalagtár egyik HTML-töredék és szerkezetek, hogy tooXSS támadások okozhat a dokumentumokat. AngleSharp tooparse használ, módosíthatók, és HTML és CSS megjelenítéséhez. HtmlSanitizer NuGet-csomagként telepíthető és hello felhasználói bevitel is átadható a HTML- vagy CSS tisztítási módszerekről, amennyiben alkalmazható, hello kiszolgáló oldalán. Vegye figyelembe a biztonsági vezérlőként tisztítási csak utolsó lehetőségként kell figyelembe venni.</p><p>Bemenet-ellenőrzéshez és a kimeneti kódolás minősülnek jobb biztonsági vezérlők.</p> |

## <a id="inbuilt-encode"></a>Ne rendeljen DOM elemek toosinks, amely nem rendelkezik beépített kódolás

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Sok javascript-funkcióként nem a kódolást alapértelmezés szerint. Nem megbízható bemeneti tooDOM elemek ilyen funkciókon keresztül hozzárendelésekor hely parancsfájl (lehetővé) végrehajtások közötti eredményezhet.| 

### <a name="example"></a>Példa
Az alábbiakban nem biztonságos példák: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Ne használjon `innerHtml`; Ehelyett használjon `innerText`. Hasonlóképpen, ahelyett, hogy `$("#elm").html()`, használata`$("#elm").text()` 

## <a id="redirect-safe"></a>Az összes ellenőrzése hello alkalmazáson belül átirányítások lezárt vagy biztonságosan történik

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [hello OAuth 2.0 hitelesítési keretrendszer - átirányító megnyitása](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Lépések** | <p>Átirányítás tooa felhasználó által megadott helyen igénylő alkalmazás tervét hello lehetséges átirányítási célok tooa előre definiált "biztonságos" listája helyeket vagy tartományokat kell korlátozni. Minden átirányítások hello alkalmazás bezárása vagy biztonságos kell lennie.</p><p>toodo ezt:</p><ul><li>Minden átirányítások azonosítása</li><li>Minden átirányítási egy megfelelő megoldás megvalósításához. Megfelelő megoldást átirányítási engedélyezési lista vagy a felhasználó a jóváhagyás tartalmazza. Ha egy webhely vagy a szolgáltatást, amely egy nyitott átirányítási biztonsági rés használja a Facebook/OAuth/OpenID Identitásszolgáltatók, egy támadó ellopni a felhasználó bejelentkezési jogkivonatot, és, hogy a felhasználó megszemélyesítése. Ez egy járó kockázat akkor, ha a "hello OAuth 2.0 hitelesítési keretrendszer" RFC 6749 ismertetett OAuth protokollt használó, 10.15 "nyissa meg a átirányítja a felhasználókat" szakasz hasonlóan, a felhasználói hitelesítő adatok sérülhet a rendszer által célzott adathalász támadások nyitott átirányítások használatával</li></ul>|

## <a id="string-method"></a>Megvalósítása bemenet-ellenőrzést az összes karakterlánc típusú paramétert fogadja el a tartományvezérlő módszerek

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A modell adatai egy MVC alkalmazás ellenőrzése](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [irányadó elvek az ASP.NET MVC-alkalmazások](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Lépések** | Elfogadó csak primitív adattípus és nem a modell argumentumként módszerek reguláris kifejezés használatával bemenet-ellenőrzést kell végezni. Itt Regex.IsMatch használandó érvényes reguláris kifejezéssel mintával. Hello bemeneti nem egyezik a megadott reguláris kifejezés hello, vezérlő nem kell végrehajtásának folytatásához és egy sikertelen ellenőrzést megfelelő figyelmeztetés üzenetnek kell megjelennie.| 

## <a id="dos-expression"></a>A reguláris kifejezés feldolgozása miatt toobad reguláris kifejezések tooprevent DoS felső korlátja időtúllépés beállítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, Web Forms, MVC5, MVC6  |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [DefaultRegexMatchTimeout tulajdonság](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Lépések** | tooensure szolgáltatásmegtagadási támadások elleni rosszul létrehozott reguláris kifejezések, nagy mennyiségű meglátogassa okozó beállítani hello globális alapértelmezett időkorlátját. Hello feldolgozási időt hello definiált felső korlátja hosszabb időt vesz igénybe, ha azt szeretné throw egy időtúllépési kivétel. Ha semmi van konfigurálva, hello időtúllépés végtelen lesz.| 

### <a name="example"></a>Példa
Például hello következő konfigurációs kivételhibát egy RegexMatchTimeoutException, ha 5 másodpercnél a hello feldolgozásra kerül: 

```C#
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Kerülje a Html.Raw Razor nézetekben

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Lépés | ASP.Net-weblapok (Razor) kódolásra automatikus HTML. Beágyazott kód nuggets (@ blokkolja) által nyomtatott összes értékek a következők automatikusan HTML-kódolású. Azonban ha `HtmlHelper.Raw` metódus meghívták, visszatérési kód, amely nincs HTML-kódolásban. Ha `Html.Raw()` segítő módszert használ, hello automatikus kódolási védelmi Razor biztosító kihagyva.|

### <a name="example"></a>Példa
Az alábbiakban egy nem biztonságos példa látható: 

```C#
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Ne használjon `Html.Raw()` kivéve toodisplay markup van szüksége. Ez a módszer nem hajt végre implicit módon kódolás kimenete. Más ASP.NET segítő például használata`@Html.DisplayFor()` 

## <a id="stored-proc"></a>Ne használjon dinamikus lekérdezések a tárolt eljárások

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Egy SQL injektálási támadás bemenet-ellenőrzéshez toorun tetszőleges parancsok hello adatbázis a biztonsági kihasználja. Azt akkor fordulhat elő, amikor az alkalmazás által bemeneti tooconstruct dinamikus SQL-utasítások tooaccess hello adatbázis. Ez akkor is előfordulhat, ha a kódot használja átadott nyers felhasználói bevitel karakterláncokat, tárolt eljárások. Hello SQL injektálási támadás használ, a támadó hello tetszőleges parancsot hello adatbázis végrehajthat. Az összes SQL-utasítások (beleértve a hello SQL-utasítások a tárolt eljárások) kell paraméteres. A paraméteres SQL-utasítások karakterek, amelyek rendelkeznek speciális jelentéssel tooSQL (például az aposztróf) hiba nélkül, mert azok erős típusmegadású fogad el. |

### <a name="example"></a>Példa
Az alábbiakban nem biztonságos dinamikus tárolt eljárás egy példa látható: 

```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Példa
Az alábbiakban olvashat ugyanazon tárolt eljárás végrehajtása biztonságosan hello: 
```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a id="validation-api"></a>Arról, hogy Modellellenőrzés a webes API-módszer

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az ASP.NET Web API Modellellenőrzés](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Lépések** | Amikor egy ügyfél küld adatokat tooa webes API-t, akkor kötelező toovalidate hello adatok minden elvégzése előtt. Az ASP.NET Web API-kat elfogadó modellek bemenetként modellek tooset ellenőrzési szabályok hello tulajdonságainál hello modell adatok jegyzetek használatát.|

### <a name="example"></a>Példa
hello a következő kód bemutatja, hogy hello azonos: 

```C#
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Példa
Hello tartozó műveleti módszer hello API vezérlők hello modell érvényességét rendelkezik toobe explicit módon be van jelölve a lent látható módon: 

```C#
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with hello product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a id="string-api"></a>Megvalósítása bemenet-ellenőrzést az összes karakterlánc típusú paraméterek fogadja el a webes API-módszer

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, MVC 5, 6 MVC |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A modell adatai egy MVC alkalmazás ellenőrzése](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [irányadó elvek az ASP.NET MVC-alkalmazások](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Lépések** | Elfogadó csak primitív adattípus és nem a modell argumentumként módszerek reguláris kifejezés használatával bemenet-ellenőrzést kell végezni. Itt Regex.IsMatch használandó érvényes reguláris kifejezéssel mintával. Hello bemeneti nem egyezik a megadott reguláris kifejezés hello, vezérlő nem kell végrehajtásának folytatásához és egy sikertelen ellenőrzést megfelelő figyelmeztetés üzenetnek kell megjelennie.|

## <a id="typesafe-api"></a>Ellenőrizze, hogy biztonságos típusú paraméterek szolgáltatás webes API-ban az adatelérési

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Hello paramétergyűjtemény használatakor SQL kezeli hello bemeneti van, mint végrehajtható kód, nem konstans érték. hello paramétergyűjtemény lehet használt tooenforce típusa és hossza korlátozza a bemeneti adatok. Hello tartományon kívül eső értékeket kivételt vált. Típus környezetben is biztonságos SQL-paraméterek nem használják, ha a támadók lehet szűrés hello bemeneti beágyazott képes tooexecute injektálási támadások.</p><p>Felhasználhatja a biztonságos Típusparaméterek tooavoid lehetséges SQL injektálási támadásokkal szemben, amelyek alakulhat ki nem szűrt bemeneti hozhat létre, SQL lekérdezések. Biztonságos Típusparaméterek is használhatja, tárolt eljárások és a dinamikus SQL-utasításokat. Paraméterek literálértékek hello adatbázis által, nem pedig végrehajtható kód kell kezelni. Paraméterek típusa és hossza is ellenőrzi.</p>|

### <a name="example"></a>Példa
hello következő kód bemutatja, hogyan toouse adja-e biztonságos paramétereket hello SqlParameterCollection tárolt eljárás meghívásakor. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Az előző példakódban hello hello bemeneti érték nem lehet hosszabb, mint 11 karakter. Ha hello adat nem felel meg a toohello típus vagy hello paraméter által meghatározott, hello SqlParameter osztály kivételt jelez. 

## <a id="sql-docdb"></a>Cosmos DB paraméteres SQL-lekérdezések használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Document DB rendszerbe | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A documentdb SQL Parameterization bejelentése](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Lépések** | Bár a DocumentDB csak olvasási lekérdezéseket támogat, SQL-injektálás továbbra is lehetséges, ha a lekérdezések össze a felhasználói bevitel szereplő. Lehetséges, egy felhasználó toogain hozzáférés toodata érik nem lehet hello belül a rosszindulatú SQL-lekérdezések létrehozásával ugyanahhoz a gyűjteményhez. Használja a paraméteres SQL-lekérdezések Ha lekérdezések össze a felhasználói bevitel alapján. |

## <a id="schema-binding"></a>WCF bemeneti érvényesítési séma kötését keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Lépések** | <p>Érvényesítési hiánya toodifferent típusú injektálási támadások vezet.</p><p>Üzenet érvényesítési védelmi hello védelmi a WCF-alkalmazás egy vonal jelöli. Ezt a módszert sémák tooprotect WCF szolgáltatási műveleteket egy rosszindulatú ügyfél támadások elleni üzenetek ellenőrzése. Egy rosszindulatú szolgáltatás hello ügyfél tooprotect hello ügyfél támadások elleni által fogadott összes üzenetek ellenőrzése. Üzenet érvényesítési teszi lehetséges toovalidate üzenetek műveletek üzenet szerződések és nem lehet végrehajtani, adategyezményeinek használni, amikor paraméter-érvényesítés használatával. Üzenet érvényesítési lehetővé teszi a toocreate ellenőrzési logika sémákat, ezáltal nagyobb rugalmasságot biztosít, és fejlesztési időn belül. Sémák belül hello szervezet létrehozása szabványokkal adatok ábrázolása a különböző alkalmazások különböző felhasználhatók. Emellett üzenet érvényesítési lehetővé teszi tooprotect műveletek amikor összetettebb adattípusok használata esetén az üzleti logika képviselő szerződéseket.</p><p>tooperform üzenet érvényesítési, először létre hello műveletek a szolgáltatás és hello adattípusok a műveletek által felhasznált jelölő séma. Ezután hozzon létre egy egyéni ügyfél-üzenet inspector és egyéni kézbesítő üzenetet inspector toovalidate hello küldött/fogadott üzenetek hello szolgáltatást és a .NET-osztályt. Egy egyéni végpont viselkedés tooenable üzenet érvényesítése a következő hello ügyfél és a hello szolgáltatás megvalósításához. Végezetül megvalósítása a egy egyéni konfigurációs elem hello osztály, amely lehetővé teszi, hogy tooexpose hello kiterjesztett egyéni végpontviselkedéshez hello szolgáltatást vagy hello ügyfél hello konfigurációs fájlban az"</p>|

## <a id="parameters"></a>WCF - bemenet érvényesítési paraméter ellenőrök keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Lépések** | <p>Bemeneti és adatérvényesítés védelmet hello védelmét a WCF-alkalmazás, egy fontos üzletági jelöli. Ellenőriznie kell a WCF szolgáltatást műveletek tooprotect hello szolgáltatásban támadások elleni rosszindulatú ügyfél által elérhetővé tett összes paramétert. Ezzel ellentétben a hello ügyfél tooprotect hello ügyfél támadások elleni egy rosszindulatú szolgáltatás által fogadott összes visszatérési értékek is ellenőrizni kell</p><p>WCF, amelyek lehetővé teszik egyéni kiterjesztések létrehozásával toocustomize hello WCF működését különböző bővítési pontokat biztosít. Üzenet ellenőrök és paraméter ellenőrök két bővítési mechanizmus használt toogain nagyobb mértékben vezérelheti a hello adatok egy ügyfél és a szolgáltatás között. Kell használni ellenőrök paraméter bemenet-ellenőrzéshez és üzenet ellenőrök használata csak akkor, ha tooinspect hello teljes üzenet mindkét szolgáltatás áramló van szükség.</p><p>tooperform bemeneti ellenőrzés, hogy elkészíti a .NET-osztályt és valósít meg egy egyéni paraméter-inspector rendelés toovalidate paraméterek műveletek a szolgáltatás. Egy egyéni végpont viselkedés tooenable érvényesítési majd hello ügyfél és a hello szolgáltatást fogja végrehajtani. Végezetül megvalósítandó egy egyéni konfigurációs elem hello osztály, amely lehetővé teszi, hogy tooexpose hello kiterjesztett egyéni végpontviselkedéshez hello szolgáltatást vagy hello ügyfél hello konfigurációs fájlban</p>|
