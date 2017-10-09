---
title: "az Azure Media Indexer beállított aaaTask"
description: "Ez a témakör áttekintést nyújt az Azure Media Indexer beállított feladat."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: ca0b3e7aa9f6dd9fdecddfc5b3137281ed5cef35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="task-preset-for-azure-media-indexer"></a>A feladat az Azure Media Indexer az adott néven beállítás

Az Azure Media Indexer egy adathordozó processzor, a következő feladatok tooperform hello használata: médiafájlok és a tartalom kereshető győződjön, lezárt feliratok nyomon követi és kulcsszavak létrehozása, az eszköz részét képező eszköz fájlok indexelése.

Ez a témakör ismerteti a hello feladat az adott néven beállítás, hogy kell-e toopass tooyour indexelő feladat. Teljes példa, lásd: [médiafájlok az Azure Media Indexer indexelő](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Az Azure Media Indexer konfigurációs XML-kód

hello következő táblázat ismerteti elemek és attribútumok XML-hello konfiguráció.

|Név|Kötelező|Leírás|
|---|---|---|
|Input (Bemenet)|Igaz|Eszköz-fájl, amelyet az tooindex.<br/>Az Azure Media Indexer támogatja a következő fájlformátumokat media hello: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Hello fájl neve (s) adhat meg hello **neve** vagy **lista** hello attribútumának **bemeneti** eleme (lásd alább). Ha nem adja meg a mely eszköz fájl tooindex, hello elsődleges fájl nek. Ha nem elsődleges eszköz fájl be van állítva, indexelt hello hello bemeneti eszköz első fájlt.<br/><br/>tooexplicitly hello eszköz nevét adja meg, tegye:<br/>```<input name="TestFile.wmv" />```<br/><br/>Több eszköz fájl egyszerre (fájlokról too10) is elvégezheti az indexelést. toodo ezt:<br/>– Hozzon létre egy szövegfájlt (jegyzékfájl), és adjon neki egy .lst bővítmény.<br/>-Adja meg az összes hello eszköz fájlnevek a bemeneti eszköz toothis jegyzékfájl.<br/>-Adjon hozzá (feltöltés) thanifest fájl toohello eszköz.<br/>-Adja meg hello jegyzékfájl nevének hello hello bemeneti attribútum.<br/>```<input list="input.lst">```<br/><br/>**Megjegyzés:** 10-nél több fájlok toohello jegyzékfájl való hozzáadásakor hello indexelő feladat hello 2006 hibakóddal meghiúsul.|
|Metaadatok|hamis|A megadott eszköz (oka) t hello metaadatait.<br/>```<metadata key="..." value="..." />```<br/><br/>Előre definiált kulcsok értéket ad meg. <br/><br/>Jelenleg a következő kulcsok hello támogatottak:<br/><br/>**cím** és **leírás** -használt tooupdate hello nyelvi modell tooimprove beszédfelismerés pontosságát.<br/>```<metadata key="title" value="[Title of hello media file]" /><metadata key="description" value="[Description of hello media file]" />```<br/><br/>**felhasználónév** és **jelszó** - hitelesítéshez a http vagy HTTPS protokollon keresztül internetes fájlok letöltésekor használt.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>hello felhasználónév és jelszó értékeinek hello bemeneti jegyzékben tooall media URL-címek érvényesek.|
|funkciókkal<br/><br/>A hozzáadott 1.2-es verzióját. Csak a támogatott hello szolgáltatás jelenleg Beszédfelismerés ("automatikus").|hamis|hello beszédfelismerés szolgáltatás a következő beállítások kulcsok hello rendelkezik:<br/><br/>Nyelv:<br/>-hello természetes nyelvű toobe felismerhető hello multimédiás fájlban.<br/>-Angol, spanyol<br/><br/>CaptionFormats:<br/>-hello pontosvesszővel elválasztott listája szükséges felirat formátumokban (ha van ilyen)<br/>-ttml; Számi; webvtt<br/><br/><br/>GenerateAIB:<br/>-A logikai jelző, adja meg, hogy-e egy AIB fájl szükséges (az SQL Server és hello ügyfél indexelő IFilter való használatra). További információkért lásd: az Azure Media Indexer és az SQL Server használatával AIB fájlok.<br/>-Igaz; Hamis<br/><br/>GenerateKeywords:<br/>-A logikai jelző, adja meg, hogy-e a kulcsszó XML-fájl szükséges.<br/>-Igaz; Hamis.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Az Azure Media Indexer konfigurációs XML-példa

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of hello media file]" />  
    <metadata key="description" value="[Description of hello media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="CaptionFormats" value="ttml;sami;webvtt"/>  
        <add key="GenerateAIB" value ="true" />  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Következő lépések

Lásd: [médiafájlok az Azure Media Indexer indexelő](media-services-index-content.md).

