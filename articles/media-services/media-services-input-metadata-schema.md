---
title: "aaaAzure Media Services bemeneti metaadatok séma |} Microsoft Docs"
description: "hello a témakör áttekintést nyújt az Azure Media Services bemeneti metaadatok séma."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 9b72c6ff317aa98451ea75548465dc6023b44a55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="input-metadata"></a>Bemeneti metaadatok
A kódolási feladat vagy társítva egy bemeneti eszköz (eszközök) kívánja tooperform egyes kódolási feladatokhoz.  Létrehozása után egy feladatot kimeneti eszközként hozott létre.  hello kimeneti eszköz tartalmaz, illetve hang-, videoindexképek, jegyzék, stb. hello kimeneti adategységen is tartalmaz hello bemeneti eszköz metaadatait tartalmazó fájl. hello hello metaadatok XML-fájl neve van hello a következő formátumban: &lt;asset_id&gt;_metadata.xml (például 41114ad3 eb5e - 4 - c 57-8d 92-5354e2b7d4a4_metadata.xml), ahol &lt;asset_id&gt; hello AssetId hello bemeneti eszköz értéke.  

Ha azt szeretné, hogy tooexamine hello metaadatfájl, létrehozhat egy **SAS** lokátor és a letöltési hello fájl tooyour helyi számítógépen. Egy példa található hogyan toocreate egy SAS-kereső és töltse le a fájlt [hello Media Services .NET SDK-bővítmények használatával](media-services-dotnet-get-started.md).  

Ez a témakör ismerteti a mely hello bemeneti metada hello elemek és hello XML-séma típusú (&lt;asset_id&gt;_metadata.xml) alapul.  Hello kimeneti adategységen vonatkozó metaadatok tartalmazó hello fájllal kapcsolatos információkért lásd: [kimeneti metaadatok](media-services-output-metadata-schema.md).  

> [!NOTE]
> Hello található [séma kód](media-services-input-metadata-schema.md#code) egy [XML-példa](media-services-input-metadata-schema.md#xml) hello Ez a témakör végén.  
> 
> 

## <a name="AssetFiles"></a>AssetFiles elem (legfelső szintű elem)
Gyűjteményét tartalmazza [AssetFile elem](media-services-input-metadata-schema.md#AssetFile)s hello kódolási feladat.  

Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

| Név | Leírás |
| --- | --- |
| **AssetFile**<br /><br /> minOccurs = "1" maxOccurs = "unbounded" |Egyetlen gyermekelemet. További információkért lásd: [AssetFile elem](media-services-input-metadata-schema.md#AssetFile). |

## <a name="AssetFile"></a>AssetFile elem
 Attribútumok és elemek leíró egy eszköz-fájl tartalmazza.  

 Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Name (Név)**<br /><br /> Szükséges |**xs:String** |Eszköz neve. |
| **Méret**<br /><br /> Szükséges |**xs:Long** |Mérete bájtban hello objektumfájlt. |
| **Időtartam**<br /><br /> Szükséges |**DURATION típusú** |Tartalom play hátsó időtartama. Példa: Duration = "PT25M37.757S". |
| **NumberOfStreams**<br /><br /> Szükséges |**xs:INT** |Hello objektumfájlt adatfolyamok száma. |
| **FormatNames**<br /><br /> Szükséges |**xs:String** |Formátumnevek. |
| **FormatVerboseNames**<br /><br /> Szükséges |**xs:String** |Részletes formátumnevek. |
| **Kezdő időpont** |**DURATION típusú** |Tartalom kezdete. Példa: StartTime = "PT2.669S". |
| **OverallBitRate** |**xs:INT** |Hello eszköz fájl kbit/s átlagos átviteli sebesség. |

> [!NOTE]
> a következő 4 gyermekelemek hello sorrendben kell szerepelnie.  
> 
> 

### <a name="child-elements"></a>Gyermekelemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Programok**<br /><br /> minOccurs = "0" | |Az összes gyűjtemény [programok elem](media-services-input-metadata-schema.md#Programs) hello objektumfájlt esetén MPEG-TS formátumban. |
| **VideoTracks**<br /><br /> minOccurs = "0" | |Minden egyes fizikai eszköz fájl tartalmazhat nulla vagy több videó nyomon követi a megfelelő tárolót formátumra időosztásos. Ez az elem tartalmazza az összes [VideoTracks elem](media-services-input-metadata-schema.md#VideoTracks) hello objektumfájlt részét képező. |
| **AudioTracks**<br /><br /> minOccurs = "0" | |Minden egyes fizikai eszköz fájl tartalmazhat nulla vagy több zeneszámok egy megfelelő tárolót formátumra időosztásos. Ez az elem tartalmazza az összes [AudioTracks elem](media-services-input-metadata-schema.md#AudioTracks) hello objektumfájlt részét képező. |
| **Metaadatok**<br /><br /> minOccurs = "0" maxOccurs = "unbounded" |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Eszköz fájl metaadat-ként key\value karakterláncok. Példa:<br /><br /> **&lt;A metaadat kulcsként = "nyelv" value = "hun" /&gt;** |

## <a name="TrackType"></a>TrackType
Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Azonosítója**<br /><br /> Szükséges |**xs:INT** |A hang- vagy követése nulla alapú indexét.<br /><br /> Ez nem szükségszerűen adott hello TrackID használt MP4-fájlokat. |
| **Kodek** |**xs:String** |Videó követése kodek karakterlánc. |
| **CodecLongName** |**xs:String** |Hang- vagy követése kodek hosszú neve. |
| **TimeBase**<br /><br /> Szükséges |**xs:String** |Idejének alapja. Példa: TimeBase = "1/48000" |
| **NumberOfFrames** |**xs:INT** |Keretek (videó nyomon követi a jelen) száma. |
| **Kezdő időpont** |**DURATION típusú** |Kezdési idő nyomon követése. Példa: StartTime = "PT2.669S" |
| **Időtartam** |**DURATION típusú** |Időtartam nyomon. Példa: Duration = "PTSampleFormat M37.757S". |

> [!NOTE]
> a következő 2 gyermekelemek hello sorrendben kell szerepelnie.  
> 
> 

### <a name="child-elements"></a>Gyermekelemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Törlése**<br /><br /> minOccurs = "0" maxOccurs = "1" |[StreamDispositionType](media-services-input-metadata-schema.md#StreamDispositionType) |Megjelenítési adatok (például, hogy egy adott hang nyomon követése a gyengén látó felhasználóknak) tartalmazza. |
| **Metaadatok**<br /><br /> minOccurs = "0" maxOccurs = "unbounded" |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Általános kulcs/érték karakterláncok, amelyek használt toohold számos információs lehetnek. Például a kulcs = "nyelv", és az érték = "hun". |

## <a name="AudioTrackType"></a>AudioTrackType (TrackType örököl)
 **AudioTrackType** származó globális összetett típus [TrackType](media-services-input-metadata-schema.md#TrackType).  

 hello típus hello eszköz fájlban meghatározott hang nyomon jelképezi.  

 Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **SampleFormat** |**xs:String** |A minta-formátum. |
| **ChannelLayout** |**xs:String** |Csatorna elrendezés. |
| **Csatornák**<br /><br /> Szükséges |**xs:INT** |(0 vagy több) hang csatornák száma. |
| **Érvénytelen a SamplingRate**<br /><br /> Szükséges |**xs:INT** |Hang mintavételi ráta minták másodpercenkénti vagy Hz. |
| **Átviteli sebesség** |**xs:INT** |Bit / másodperc, a kiszámított hello eszköz fájlból átlagos átviteli sebességet. Csak a hello elemi adatfolyam hasznos számít, és hello csomagolás terhelést nem szerepel a számláló értéke. |
| **A BitsPerSample** |**xs:INT** |Bit / minta hello wFormatTag formátumban írja be. |

## <a name="VideoTrackType"></a>VideoTrackType (TrackType örököl)
**VideoTrackType** származó globális összetett típus [TrackType](media-services-input-metadata-schema.md#TrackType).  

hello típus hello eszköz fájlban meghatározott videó nyomon jelképezi.  

Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **FourCC**<br /><br /> Szükséges |**xs:String** |Videó kodek FourCC kódot. |
| **Profil** |**xs:String** |Videó követése profil. |
| **Szint** |**xs:String** |Videó követése szintje. |
| **PixelFormat** |**xs:String** |Videó követése képpontformátum. |
| **Szélessége**<br /><br /> Szükséges |**xs:INT** |Kódolt videó szélessége képpontban megadva. |
| **Magassága**<br /><br /> Szükséges |**xs:INT** |Videó magassága képpontban kódolva. |
| **DisplayAspectRatioNumerator**<br /><br /> Szükséges |**xs:Double** |Képmegjelenítő oldalarányának számlálójának. |
| **DisplayAspectRatioDenominator**<br /><br /> Szükséges |**xs:Double** |Képmegjelenítő oldalarányának nevező. |
| **DisplayAspectRatioDenominator**<br /><br /> Szükséges |**xs:Double** |Videó minta oldalarányának számlálójának. |
| **SampleAspectRatioNumerator** |**xs:Double** |Videó minta oldalarányának számlálójának. |
| **SampleAspectRatioNumerator** |**xs:Double** |Videó minta oldalarányának nevező. |
| **Képkockasebességhez**<br /><br /> Szükséges |**xs:decimal** |Mért videó képkockasebessége .3f formátumban. |
| **Átviteli sebesség** |**xs:INT** |Kilobit / másodperc, a kiszámított hello eszköz fájlból videó átlagos átviteli sebességet. Csak a hello elemi adatfolyam hasznos számít, és hello csomagolás terhelés nincs megadva. |
| **MaxGOPBitrate** |**xs:INT** |Maximális GOP a videó nyomon követése, a kilobit / másodperc átlagos sávszélességű. |
| **HasBFrames** |**xs:INT** |B keretek videó követése száma. |

## <a name="MetadataType"></a>MetadataType
**MetadataType** leíró metaadatok eszköz fájlok kulcs/érték karakterláncként globális összetett típus. Például a kulcs = "nyelv", és az érték = "hun".  

Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **kulcs**<br /><br /> Szükséges |**xs:String** |hello kulcs/érték pár hello kulcs. |
| **érték**<br /><br /> Szükséges |**xs:String** |hello kulcs/érték pár hello érték. |

## <a name="ProgramType"></a>ProgramType
**ProgramType** globális összetett típus, amely leírja a program.  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **ProgramId**<br /><br /> Szükséges |**xs:INT** |Program azonosítója |
| **NumberOfPrograms**<br /><br /> Szükséges |**xs:INT** |Programok száma. |
| **PmtPid**<br /><br /> Szükséges |**xs:INT** |Program térkép táblák (PMTs) a programok információkat tartalmaznak.  További információkért lásd: [részlet](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT). |
| **PcrPid**<br /><br /> Szükséges |**xs:INT** |A dekódoló használják. További információkért lásd: [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR) |
| **StartPTS** |**xs: hosszú** |Kezdő bemutató időbélyegző. |
| **EndPTS** |**xs: hosszú** |Befejezési bemutató időbélyegző. |

## <a name="StreamDispositionType"></a>StreamDispositionType
**StreamDispositionType** hello adatfolyam leíró globális összetett típus.  

Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Alapértelmezett**<br /><br /> Szükséges |**xs:INT** |Ez ez az attribútum too1 tooindicate hello alapértelmezett bemutató beállítása. |
| **Dub**<br /><br /> Szükséges |**xs:INT** |Állítsa a attribútum too1 tooindicate van hello szinkronizált bemutató. |
| **Eredeti**<br /><br /> Szükséges |**xs:INT** |Ez ez az attribútum too1 tooindicate hello eredeti bemutató beállítása. |
| **Megjegyzés**<br /><br /> Szükséges |**xs:INT** |Állítsa a attribútum too1 tooindicate követése magyarázatokkal tartalmazza. |
| **Szöveg**<br /><br /> Szükséges |**xs:INT** |Állítsa a attribútum too1 tooindicate követése szöveget tartalmaz. |
| **Karaoke**<br /><br /> Szükséges |**xs:INT** |Ez az attribútum too1 tooindicate beállítása jelöli hello karaoke nyomon követése (háttér zene, nincs énekhez). |
| **A kényszerített**<br /><br /> Szükséges |**xs:INT** |Ez ez az attribútum too1 tooindicate kényszerített hello bemutató beállítása. |
| **HearingImpaired**<br /><br /> Szükséges |**xs:INT** |Az attribútum too1 tooindicate sérült hello nyújtanak segítséget a szám van beállítva. |
| **VisualImpaired**<br /><br /> Szükséges |**xs:INT** |Ez a szám a gyengén látó hello van attribútum too1 tooindicate beállítása. |
| **CleanEffects**<br /><br /> Szükséges |**xs:INT** |Ez a szám rendelkezik attribútum too1 tooindicate tiszta hatások beállítása. |
| **AttachedPic**<br /><br /> Szükséges |**xs:INT** |Ez a szám rendelkezik attribútum too1 tooindicate képek beállítása. |

## <a name="Programs"></a>Programok elem
Több okozó burkolóelemet **Program** elemek.  

### <a name="child-elements"></a>Gyermekelemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Program**<br /><br /> minOccurs = "0" maxOccurs = "unbounded" |[ProgramType](media-services-input-metadata-schema.md#ProgramType) |Eszköz MPEG-TS formátumú fájlok esetében hello objektumfájlt programokkal kapcsolatos információkat tartalmazza. |

## <a name="VideoTracks"></a>VideoTracks elem
 Több okozó burkolóelemet **VideoTrack** elemek.  

 Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="child-elements"></a>Gyermekelemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **VideoTrack**<br /><br /> minOccurs = "0" maxOccurs = "unbounded" |[VideoTrackType (TrackType örököl)](media-services-input-metadata-schema.md#VideoTrackType) |Videó nyomon követi a hello eszköz fájl adatait tartalmazza. |

## <a name="AudioTracks"></a>AudioTracks elem
 Több okozó burkolóelemet **AudioTrack** elemek.  

 Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).  

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **AudioTrack**<br /><br /> minOccurs = "0" maxOccurs = "unbounded" |[AudioTrackType (TrackType örököl)](media-services-input-metadata-schema.md#AudioTrackType) |Hello eszköz fájlban zeneszámok adatait tartalmazza. |

## <a name="code"></a>Séma kódot
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
        <xs:attribute name="Id" use="required">  
          <xs:annotation>  
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Width" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video width in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Height" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video height in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
              <xs:annotation>  
                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:decimal">  
                  <xs:minInclusive value="0"/>  
                  <xs:fractionDigits value="3"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="MaxGOPBitrate">  
              <xs:annotation>  
                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Channels" use="required">  
              <xs:annotation>  
                <xs:documentation>number of audio channels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SamplingRate" use="required">  
              <xs:annotation>  
                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for hello encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is hello collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>hello media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of hello asset file in kbps</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:int">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  


## <a name="xml"></a>XML-példa
hello hello bemeneti metaadatait tartalmazó fájl egy példa látható.  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

