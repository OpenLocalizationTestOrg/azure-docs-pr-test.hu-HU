---
title: "Event Hubs rögzítése forgatókönyv aaaAzure |} Microsoft Docs"
description: "A minta által használt hello Azure Python SDK toodemonstrate hello Event Hubs rögzítése funkció használata."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: darosa;sethm
ms.openlocfilehash: 1737dcca283711d863aa970db0e80ae71814e666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Event Hubs rögzítése forgatókönyv: Python

Event Hubs rögzítése egyik funkciója, amely lehetővé teszi, hogy tooautomatically Event Hubs biztosítanak az event hub tooan az Ön által választott Azure Blob storage-fiók adatainak streaming hello. Ez a funkció lehetővé teszi a valós idejű streamelési adatok könnyen tooperform kötegfeldolgozási. Ez a cikk ismerteti, hogyan toouse Event Hubs rögzítése a Python. Event Hubs rögzítése kapcsolatos további információkért lásd: hello [a cikk áttekintése](event-hubs-archive-overview.md).

Ezt a mintát használ hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello rögzítése funkció. hello sender.py program tooEvent hubok elküldi szimulált környezeti telemetriai adatok JSON formátumban. hello eseményközpont beállításai toouse hello rögzítése funkció toowrite az adattárolás tooblob kötegekben. hello capturereader.py app majd beolvassa a blobok és eszközönként egy hozzáfűző fájlt hoz létre, majd írja az hello adatokat .csv fájlok.

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja

1. Hozzon létre egy Azure Blob Storage-fiókot és egy blob tároló belül, hello Azure-portál használatával.
2. Hozzon létre egy Eseményközpontba névtér hello Azure-portál használatával.
3. Hozzon létre egy eseményközpontba hello rögzítési szolgáltatás engedélyezve van, hello Azure-portál használatával.
4. Adatok toohello event hubs egy Python-parancsfájl küldése.
5. Hello fájlok olvasásának hello rögzítési és dolgozza fel őket másik Python-parancsfájl.

## <a name="prerequisites"></a>Előfeltételek

- Python 2.7.x
- Azure-előfizetés
- Az aktív [Event Hubs névtér és az event hub.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Azure Storage-fiók létrehozása
1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **tárolási**, és kattintson a **Tárfiók**.
3. Töltse ki a storage-fiók panelen hello hello mezőket, és kattintson a **létrehozása**.
   
   ![][1]
4. Miután meggyőződött arról, hogy hello **központi telepítések sikeres** üzenetet, kattintson az új tárfiók hello és hello hello neve **Essentials** panelen kattintson a **Blobok**. Ha hello **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** hello tetején. Név hello tároló **rögzítése**, majd zárja be hello **Blob szolgáltatás** panelen.
5. Kattintson a **hívóbetűk** a hello bal oldali panelen, és másolja hello hello tárolási fiók nevét és értékét hello **key1**. Ezen értékek tooNotepad vagy más ideiglenes helyre mentse.

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a>A Python parancsfájl toosend események tooyour eseményközpont létrehozása
1. Nyissa meg a kedvenc Python-szerkesztőt, például a [Visual Studio Code][Visual Studio Code].
2. Hozzon létre nevű parancsfájl **sender.py**. Ez a parancsfájl 200 események tooyour eseményközpont küld. Elküldött JSON egyszerű környezeti értékek.
3. Illessze be a következő kódot a sender.py hello:
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. Hello megelőző kód toouse, a névtér nevét, a kulcs értékét és a eseményközpont neveként beolvasott hello Event Hubs névtér létrehozása után frissítse.

## <a name="create-a-python-script-tooread-your-capture-files"></a>Hozzon létre egy Python-parancsfájl tooread a rögzítési fájlok

1. Töltse ki a hello panelt, és kattintson a **létrehozása**.
2. Hozzon létre nevű parancsfájl **capturereader.py**. Ez a parancsfájl beolvassa a hello rögzített fájlok, és létrehoz egy fájlt egy eszköz toowrite hello adatok kizárólag az eszköznek.
3. Illessze be a következő kódot a capturereader.py hello:
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          if blob.properties.content_length != 0:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Lehet, hogy toopaste hello megfelelő értékeket a tárfiók nevét és a kulcsot a hello hívás túl`startProcessing`.

## <a name="run-hello-scripts"></a>Hello parancsfájlok futtatása
1. Nyisson meg egy parancssort, a Python a elérési útvonalát a, és futtassa a parancsokat tooinstall Python-csomagokat:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Ha egy korábbi vagy az azure-tároló, vagy az azure, szükség lehet a toouse hello **--frissítése** beállítás
   
  Szükség esetén toorun hello követve (a legtöbb rendszeren nem szükséges):
   
  ```
  pip install cryptography
  ```
2. Módosítsa a directory toowherever sender.py és capturereader.py mentette, majd futtassa ezt a parancsot:
   
  ```
  start python sender.py
  ```
   
  Egy új Python folyamat toorun hello küldő indítja el.
3. Most Várjon néhány percet a hello rögzítési toorun. Írja be a következő parancsot az eredeti parancs ablakba hello:
   
   ```
   python capturereader.py
   ```

   A rögzítési processzor használ hello helyi könyvtár toodownload hello fiók vagy a tároló az összes hello BLOB. Feldolgozza azokat, amelyek nem üres, és kiírja a hello eredményeket csv-fájlként hello helyi könyvtárba.

## <a name="next-steps"></a>Következő lépések

További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Az Event hubs – áttekintés rögzítése][Overview of Event Hubs Capture]
* [Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs].
* Hello [esemény feldolgozása az Event Hubs kibővítési] [ Scale out Event Processing with Event Hubs] minta.
* [Event Hubs – áttekintés][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
