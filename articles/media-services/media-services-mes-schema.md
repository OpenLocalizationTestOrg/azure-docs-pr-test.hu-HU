---
title: "aaaMedia Encoder Standard séma |} Microsoft Docs"
description: "hello a témakör áttekintést hello Media Encoder Standard séma."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 82bad27b9546f75557ac691ff148b46990647632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard séma
Ez a témakör néhány hello elemek és hello XML-séma típusú amelyen [Media Encoder Standard készletek](media-services-mes-presets-overview.md) alapulnak. hello témakör minden elemek és az érvényes értékekre ismertetése. hello teljes séma közzé lesz téve egy későbbi időpontban.  

## <a name="Preset"></a>Előre definiált (legfelső szintű elem)
Meghatározza az egy kódolási beállításkészlet.  

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Kódolás** |[Kódolás](media-services-mes-schema.md#Encoding) |Legfelső szintű elem azt jelzi, hogy hello bemeneti forrás toobe kódolású. |
| **Kimenetek** |[Kimenetek](media-services-mes-schema.md#Output) |Kívánt kimeneti fájlok gyűjteménye. |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Verzió**<br/><br/> Szükséges |**xs:decimal** |hello előre definiált verziót. hello alábbi korlátozások érvényesek: xs:fractionDigits érték = "1" és a xs:minInclusive érték például = "1" **verzió = "1.0"**. |

## <a name="Encoding"></a>Kódolás
A következő elemek hello sorozatát tartalmazza.  

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Videó H.264 kódolás beállításait. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |A hang kódolását AAC beállításait. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Bmp lemezképek beállításait. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Png-lemezképek beállításait. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Jpg lemezképek beállításait. |

## <a name="H264Video"></a>H264Video
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs = "0" |**xs:Boolean** |Jelenleg csak egy pass kódolás esetén támogatott. |
| **KeyFrameInterval**<br/><br/> minOccurs = "0"<br/><br/> **alapértelmezett = "00: 00:02"** |**xs:time** |Meghatározza, hogy hello rögzített IDR keretek egységekben másodperc közötti térköz. Más néven tooas hello GOP időtartama. Lásd: **SceneChangeDetection** (alatti) e hello szabályozásának kódoló is eltérnek ezt az értéket. |
| **SceneChangeDetection**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "false" |**xs:Boolean** |Ha set tootrue, toodetect helyszín hello videó módosítsa, majd szúrja be egy IDR keret kódoló kísérletek. |
| **Összetettsége**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "Kiegyensúlyozott" |**xs:String** |Vezérlők hello kompromisszum közötti sebesség és a videó kódolása. Hello a következő értékek egyike lehet: **sebesség**, **kiegyensúlyozott**, vagy **minősége**<br/><br/> Alapértelmezett: **elosztott terhelésű** |
| **SyncMode**<br/><br/> minOccurs = "0" | |A szolgáltatás a későbbi kiadásokban megjelenik. |
| **H264Layers**<br/><br/> minOccurs = "0" |[H264Layers](media-services-mes-schema.md#H264Layers) |A réteggyűjteménynek kimeneti videó. |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Az állapot** |**xs:String** | Ha hello bemeneti nincs videó rendelkezik, érdemes lehet tooforce hello kódoló tooinsert fekete-fehér videó nyomon. toodo használó, feltétel = "InsertBlackIfNoVideoBottomLayerOnly" (tooinsert, csak hello legalacsonyabb sávszélességű videó) vagy a feltétel = "InsertBlackIfNoVideo" (tooinsert, az összes kimeneti bitrates videó). További információ [ebben](media-services-advanced-encoding-with-mes.md#no_video) a témakörben érhető el.|

## <a name="H264Layers"></a>H264Layers

Alapértelmezés szerint ha egy bemeneti toohello kódoló csak hang, és a nem kép tartalmazó küld hello kimeneti adategységen fogja tartalmazni fájlok, amelyek csak a hang adatokat. Néhány lejátszó nem lehet ilyen toohandle kimenetét. Használhatja a hello H264Video **InsertBlackIfNoVideo** egy videó követése toohello kimeneti tooforce hello kódoló tooadd beállítása az adott forgatókönyv attribútum. További információ [ebben](media-services-advanced-encoding-with-mes.md#no_video) a témakörben érhető el.
              
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs = "0" maxOccurs = "unbounded" |[H264Layer](media-services-mes-schema.md#H264Layer) |A réteggyűjteménynek H264. |

## <a name="H264Layer"></a>H264Layer
> [!NOTE]
> Videó korlátok hello ismertetett hello értékek alapuló [H264 szintek](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tábla.  
> 
> 

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "Auto" |**xs:String** |Hello következő lehetnek **xs:string** értékek: **automatikus**, **alapterv**, **fő**, **magas**. |
| **Szint**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "Auto" |**xs:String** | |
| **Átviteli sebesség**<br/><br/> minOccurs = "0" |**xs:INT** |Ez a videó réteg, Kbit/s megadott használt hello sávszélességű. |
| **MaxBitrate**<br/><br/> minOccurs = "0" |**xs:INT** |hello maximális átviteli sebesség a videó réteg, Kbit/s megadott használt. |
| **BufferWindow**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "00: 00:05" |**xs:time** |Hello videó puffer hossza. |
| **Szélessége**<br/><br/> minOccurs = "0" |**xs:INT** |Hello kimeneti videó keret, képpontban szélességét.<br/><br/> Vegye figyelembe, hogy jelenleg, meg kell adnia a szélességét és magasságát. hello szélességét és magasságát kell toobe páros számra. |
| **Magassága**<br/><br/> minOccurs = "0" |**xs:INT** |Hello kimeneti videó keret, képpontban magasságát.<br/><br/> Vegye figyelembe, hogy jelenleg, meg kell adnia a szélességét és magasságát. hello szélességét és magasságát kell toobe páros számra.|
| **BFrames**<br/><br/> minOccurs = "0" |**xs:INT** |B keretek között hivatkozás keretek számát. |
| **ReferenceFrames**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "3" |**xs:INT** |Egy GOP hivatkozás keretek számát. |
| **EntropyMode**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "Cabac" |**xs:String** |Hello a következő értékek egyike lehet: **Cabac** és **Cavlc**. |
| **Képkockasebességhez**<br/><br/> minOccurs = "0" |Racionális szám |Meghatározza, hogy hello kimeneti videó hello sebessége. A "0 vagy 1" toolet hello kódoló használata hello azonos keret értékelje, hello bemeneti videó alapértelmezett használja. Engedélyezett értékek a következők várt toobe közös képkockák díjszabás alább látható módon. Azonban minden érvényes ésszerű engedélyezett. Például 1/1 1 fps és érvényes.<br/><br/> – 12/1 (12 fps)<br/><br/> -15/1 (15 fps)<br/><br/> -24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> -25/1 (25 fps)<br/><br/>  -30/1 (30 fps)<br/><br/> 30000/1001 (29,97 fps) <br/> <br/>**Megjegyzés:** többszörös sávszélességű kódolási beállításkészlet egyéni létrehozásakor, majd minden rétegének hello beállított **kell** használata hello azonos képkockasebességhez értékét.|
| **AdaptiveBFrame**<br/><br/> minOccurs = "0" |**xs:Boolean** |Az Azure media encoder másolása |
| **A szeletek**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "0" |**xs:INT** |Meghatározza, hogy hány szeletek keret van osztva. Javasoljuk, hogy használja az alapértelmezett. |

## <a name="AACAudio"></a>AACAudio
 A következő elemek hello sorozatát tartalmaz, és csoportokat.  

 AAC kapcsolatos további információkért lásd: [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs = "0"<br/><br/> alapértelmezett = "AACLC" |**xs:String** |Hello a következő értékek egyike lehet: **AACLC**, **HEAACV1**, vagy **HEAACV2**. |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Az állapot** |**xs:String** |tooforce hello kódoló tooproduce Ha bemeneti nincsenek hang-, hang csendes nyomon tartalmazó hello "InsertSilenceIfNoAudio" értéket adjon meg.<br/><br/> Alapértelmezés szerint ha egy bemeneti toohello kódoló csak videó, és nincsenek hang tartalmazó küld majd hello kimeneti adategységen fogja tartalmazni csak videó adatokat tartalmazó fájlt. Néhány lejátszó nem lehet ilyen toohandle kimenetét. Ez a beállítás tooforce hello kódoló tooadd egy csendes hang követése toohello kimeneti forgatókönyv használható. |

### <a name="groups"></a>Csoportok
| Referencia | Leírás |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs = "0" |Leírása az [AudioGroup](media-services-mes-schema.md#AudioGroup) tooknow hello megfelelő számú csatornák mintavételi ráta és beállíthatja az egyes profilok átviteli sebességet. |

## <a name="AudioGroup"></a>AudioGroup
Milyen értékek érvényesek az egyes profilok kapcsolatos részletekért lásd: alábbi hello "Hang kodek részletei" táblázat.  

### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **Csatornák**<br/><br/> minOccurs = "0" |**xs:INT** |hang csatornák kódolású hello száma. hello az alábbiakban az érvényes beállítások: 1, 2, 5, 6, 8.<br/><br/> Alapértelmezett: 2. |
| **Érvénytelen a SamplingRate**<br/><br/> minOccurs = "0" |**xs:INT** |hello hang mintavételi ráta, Hz szerepel. |
| **Átviteli sebesség**<br/><br/> minOccurs = "0" |**xs:INT** |hello használható, ha a kódolás hello hang megadott kbit/s átviteli sebesség. |

### <a name="audio-codec-details"></a>Hang kodek részletei
Hang kodek|Részletek  
-----------------|---  
**AACLC**|1:<br/><br/> -11025: 8 &lt;sávszélességű = &lt; 16<br/><br/> -12000: 8 &lt;sávszélességű = &lt; 16<br/><br/> -16000: 8 &lt;sávszélességű = &lt;32<br/><br/>-22050: 24 &lt;sávszélességű = &lt; 32<br/><br/> -24000: 24 &lt;sávszélességű = &lt; 32<br/><br/> -32000: 32 &lt;sávszélességű = &lt;= 192<br/><br/> -44100: 56 &lt;sávszélességű = &lt;= 288<br/><br/> -48000: 56 &lt;sávszélességű = &lt;= 288<br/><br/> -88200: 128 &lt;sávszélességű = &lt;= 288<br/><br/> -96000: 128 &lt;sávszélességű = &lt;= 288<br/><br/> 2:<br/><br/> -11025: 16 &lt;sávszélességű = &lt; 24<br/><br/> -12000: 16 &lt;sávszélességű = &lt; 24<br/><br/> -16000: 16 &lt;sávszélességű = &lt; 40<br/><br/> -22050: 32 &lt;sávszélességű = &lt; 40<br/><br/> -24000: 32 &lt;sávszélességű = &lt; 40<br/><br/> -32000: 40 &lt;sávszélességű = &lt;= 384<br/><br/> -44100: 96 &lt;sávszélességű = &lt;= 576<br/><br/> -48000: 96 &lt;sávszélességű = &lt;= 576<br/><br/> -88200: 256 &lt;sávszélességű = &lt;= 576<br/><br/> -96000: 256 &lt;sávszélességű = &lt;= 576<br/><br/> 5/6:<br/><br/> -32000: 160 &lt;sávszélességű = &lt;= 896<br/><br/> -44100: 240 &lt;sávszélességű = &lt;= 1024<br/><br/> -48000: 240 &lt;sávszélességű = &lt;= 1024<br/><br/> -88200: 640 &lt;sávszélességű = &lt;= 1024<br/><br/> -96000: 640 &lt;sávszélességű = &lt;= 1024<br/><br/> 8:<br/><br/> -32000: 224 &lt;sávszélességű = &lt;= 1024<br/><br/> -44100: 384 &lt;sávszélességű = &lt;= 1024<br/><br/> -48000: 384 &lt;sávszélességű = &lt;= 1024<br/><br/> -88200: 896 &lt;sávszélességű = &lt;= 1024<br/><br/> -96000: 896 &lt;sávszélességű = &lt;= 1024  
**HEAACV1**|1:<br/><br/> -22050: sávszélességű = 8<br/><br/> -24000: 8 &lt;sávszélességű = &lt;= 10<br/><br/> -32000: 12 &lt;sávszélességű = &lt;= 64<br/><br/> -44100: 20 &lt;sávszélességű = &lt;= 64<br/><br/> -48000: 20 &lt;sávszélességű = &lt;= 64<br/><br/> -88200: sávszélességű = 64<br/><br/> 2:<br/><br/> -32000: 16 &lt;sávszélességű = &lt;= 128<br/><br/> -44100: 16 &lt;sávszélességű = &lt;= 128<br/><br/> -48000: 16 &lt;sávszélességű = &lt;= 128<br/><br/> -88200: 96 &lt;sávszélességű = &lt;= 128<br/><br/> -96000: 96 &lt;sávszélességű = &lt;= 128<br/><br/> 5/6:<br/><br/> -32000: 64 &lt;sávszélességű = &lt;= 320<br/><br/> -44100: 64 &lt;sávszélességű = &lt;= 320<br/><br/> -48000: 64 &lt;sávszélességű = &lt;= 320<br/><br/> -88200: 256 &lt;sávszélességű = &lt;= 320<br/><br/> -96000: 256 &lt;sávszélességű = &lt;= 320<br/><br/> 8:<br/><br/> -32000: 96 &lt;sávszélességű = &lt;= 448<br/><br/> -44100: 96 &lt;sávszélességű = &lt;= 448<br/><br/> -48000: 96 &lt;sávszélességű = &lt;= 448<br/><br/> -88200: 384 &lt;sávszélességű = &lt;= 448<br/><br/> -96000: 384 &lt;sávszélességű = &lt;= 448  
**HEAACV2**|2:<br/><br/> -22050: 8 &lt;sávszélességű = &lt;= 10<br/><br/> -24000: 8 &lt;sávszélességű = &lt;= 10<br/><br/> -32000: 12 &lt;sávszélességű = &lt;= 64<br/><br/> -44100: 20 &lt;sávszélességű = &lt;= 64<br/><br/> -48000: 20 &lt;sávszélességű = &lt;= 64<br/><br/> -88200: 64 &lt;sávszélességű = &lt;= 64  
  

## <a name="Clip"></a>Klip
### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Kezdő időpont** |**DURATION típusú** |A bemutató hello kezdési idejét határozza meg. StartTime hello értékének kell toomatch hello abszolút időbélyegeket hello bemeneti videó. Például ha hello hello bemeneti videó első keret 12:00:10.000 időbélyeg, akkor az StartTime kell lennie, mint 12:00:10.000 vagy nagyobb. |
| **Időtartam** |**DURATION típusú** |A bemutató (például egy átmeneti területre hello videó megjelenésének) hello időtartamát határozza meg. |

## <a name="Output"></a>Kimeneti
### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Fájlnév** |**xs:String** |hello hello kimeneti fájl nevét.<br/><br/> A következő tábla toobuild hello kimeneti fájl neve hello ismertetett makrók használatával. Példa:<br/><br/> **"Kimenetek": [{"Fájlnevet": "{Basename}*{feloldási}*{sávszélességű} .mp4", "Formátum": {"Type": "MP4Format"}}] ** |

### <a name="macros"></a>Makrók
| A makróban | Leírás |
| --- | --- |
| **{Basename}** |Akkor használatos, ha VoD kódolását, a hello {Basename} van hello hello AssetFile.Name tulajdonság hello hello bemeneti eszköz elsődleges fájl első 32 karakter.<br/><br/> Ha hello bemeneti eszköz élő archívum létrehozása, majd hello {Basename} eredete hello trackName attribútumok hello server jegyzékben. Ha egy subclip feladat hello TopBitrate, mint a elküldése: "< VideoStream\>TopBitrate < / VideoStream\>", és hello kimeneti fájl videót tartalmaz, akkor hello {Basename} van hello hello trackName hello az első 32 karakterét hello legmagasabb sávszélességű videó réteget.<br/><br/> Ha ehelyett küld egy subclip feladat összes hello bemeneti bitrates, például "< VideoStream\>* < / VideoStream\>", és hello kimeneti fájl videót tartalmaz, akkor {Basename} hello először a hello trackName, 32 karakter hosszú lehet hello megfelelő videó réteget. |
| **{Kodek}** |Túl leképezhető videó "H264" és "AAC" Audio. |
| **{Sávszélességű}** |hello cél videó sávszélességű Ha hello kimeneti fájl tartalmazza a videó és hang, vagy a cél hang sávszélességű hogy hello kimeneti fájl tartalmazza-e csak hang. hello használt értéke hello sávszélességű kbit/s. |
| **{Csatorna}** |Ha hello fájl tartalmaz hangot hang csatorna száma. |
| **{Width}** |Videó, képpontban hello kimeneti fájl, ha hello fájl tartalmaz videó hello szélességét. |
| **{Magasság}** |Videó, képpontban hello kimeneti fájl, ha hello fájl tartalmaz videó hello magasságát. |
| **{Bővítmény}** |Hello "Type" tulajdonság hello kimeneti fájl örököl. hello kimeneti fájl nevét egy bővítmény egyik lesz a: "mp4", "ts", "jpg", "png" vagy "bmp". |
| **{Index}** |Kötelező a miniatűr. Csak szerepelnie kell egyszer. |

## <a name="Video"></a>Videó (kodek örököl összetett típus)
### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Kezdés** |**xs:String** | |
| **Lépés** |**xs:String** | |
| **Tartomány** |**xs:String** | |
| **PreserveResolutionAfterRotation** |**xs:Boolean** |Részletes leírását lásd: a következő szakasz hello: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a>PreserveResolutionAfterRotation
Toouse hello PreserveResolutionAfterRotation jelzővel együtt ajánlott megoldás értékekkel százalékos értékben kifejezve (Width = "100 %", magasság = "100 %").  

Alapértelmezés szerint a hello feloldási beállításaitól (szélesség, magasság) kódolása a Media Encoder Standard (MES) készletek célzott 0 fokos Elforgatás videókat hello. Például, ha a bemeneti videó 1280 x 720 a nulla fokos elforgatás, majd hello alapértelmezett készletek ügyeljen arra, hogy hello kimeneti hello azonos megoldás. Lásd az alábbi képet.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Azonban ez azt jelenti, hogy ha a bemeneti videó hello rögzítésének nem nulla forgatási együtt (pl.. egy okostelefonján vagy táblagépén tárolt függőleges), majd MES alapértelmezés szerint települ hello feloldási beállítások (szélesség, magasság) toohello bemeneti videó, és ezután ellensúlyozza a hello Elforgatás kódolása. Például lásd az alábbi hello képet. hello készletet használ Width = "100 %", magasság = "100 %", amely MES hello kimeneti toobe 1280 széles és 720 képpontok magas igénylő értelmezi. Után hello videó elforgatása, akkor majd zsugorítja hello kép toofit be ezt az ablakot, vezető toopillar beépített területek hello a bal és jobb.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Ha a fenti hello nem szükséges hello viselkedést, akkor hello PreserveResolutionAfterRotation jelző használatát, és állítsa be úgy a túl "true" (alapértelmezett érték a "false"). Így ha az előre definiált szélessége = "100 %", magasság = "100 %", és állítsa túl "true" és egy bemeneti videó, amely 1280 széles és 720 képpontok magas a 90 fokos Elforgatás a művelet létrehoz egy kimeneti nulla fokos elforgatás, de 720 képpont széles és 1280 PreserveResolutionAfterRotation magassága képpontban megadva. Lásd az alábbi hello képet.  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a>FormatGroup (csoport)
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Elem
| Név | Típus | Leírás |
| --- | --- | --- |
| **Szélessége**<br/><br/> minOccurs = "0" |**xs:INT** | |
| **Magassága**<br/><br/> minOccurs = "0" |**xs:INT** | |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Az állapot** |**xs:String** | |

## <a name="PngLayer"></a>PngLayer
### <a name="element"></a>Elem
| Név | Típus | Leírás |
| --- | --- | --- |
| **Szélessége**<br/><br/> minOccurs = "0" |**xs:INT** | |
| **Magassága**<br/><br/> minOccurs = "0" |**xs:INT** | |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Az állapot** |**xs:String** | |

## <a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Elem
| Név | Típus | Leírás |
| --- | --- | --- |
| **Szélessége**<br/><br/> minOccurs = "0" |**xs:INT** | |
| **Magassága**<br/><br/> minOccurs = "0" |**xs:INT** | |
| **Minőségi**<br/><br/> minOccurs = "0" |**xs:INT** |Érvényes értékek: 1(worst)-100(best) |

### <a name="attributes"></a>Attribútumok
| Név | Típus | Leírás |
| --- | --- | --- |
| **Az állapot** |**xs:String** | |

## <a name="PngLayers"></a>PngLayers
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs = "0" maxOccurs = "unbounded" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs = "0" maxOccurs = "unbounded" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a>JpgLayers
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs = "0" maxOccurs = "unbounded" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a>BmpImage (videó örököl összetett típus)
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG rétegek |

## <a name="JpgImage"></a>JpgImage (videó örököl összetett típus)
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG rétegek |

## <a name="PngImage"></a>PngImage (videó örököl összetett típus)
### <a name="elements"></a>Elemek
| Név | Típus | Leírás |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG rétegek |

## <a name="examples"></a>Példák
XML-készletek, beépített példákat lásd: a séma alapján lásd [feladat készletet (Media Encoder Standard) MES](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

