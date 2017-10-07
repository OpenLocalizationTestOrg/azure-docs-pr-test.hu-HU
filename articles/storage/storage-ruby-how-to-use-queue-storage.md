---
title: a Queue storage a Ruby aaaHow toouse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure Queue szolgáltatás toocreate és a delete várólisták és a Beszúrás, get, és törli az üzenetet. Ruby írt minták."
services: storage
documentationcenter: ruby
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 726c7d2f08b2d5938ee5f9dcdc2735e447388856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a>Hogyan toouse a Queue storage a Rubyhoz
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Queue Storage szolgáltatás. hello minták hello Ruby Azure API segítségével készül.
hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-alkalmazás létrehozása
Ruby-alkalmazás létrehozása. Útmutatásért lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Alkalmazás tooAccess tároló konfigurálása
az Azure storage toouse, és van szükség toodownload használata hello Ruby azure csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-rubygems-tooobtain-hello-package"></a>RubyGems tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).
2. Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.

### <a name="import-hello-package"></a>Hello csomag importálása
Kedvenc szövegszerkesztőjével használatához adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Az Azure Storage-kapcsolat beállítása
hello azure modul hello környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_ACCESS_KEY** a információ tooconnect tooyour Azure storage-fiók szükséges. Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::QueueService** a hello a következő kódot:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a kívánt toouse toohello tárfiók.
3. Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.
4. Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot. Ezek bármelyikét használhatja. 
5. Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra. 

Ezeket az értékeket a klasszikus tárolási funkciókat biztosító fiókot a klasszikus Azure portálon hello tooobtain:

1. Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a kívánt toouse toohello tárfiók.
3. Kattintson a **ELÉRÉSI kulcsok kezelése** hello aljához hello navigációs ablaktáblán.
4. A felugró párbeszédpanel hello látni fogja a hello tárfiók neve, az elsődleges elérési kulcsát és a másodlagos elérési kulcsát. Hozzáférési kulcs hello elsődleges vagy másodlagos is hello is használhatja. 
5. Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.

## <a name="how-to-create-a-queue"></a>Útmutató: A várólista létrehozása
hello alábbi kód létrehoz egy **Azure::QueueService** objektum, amely lehetővé teszi az üzenetsorok toowork.

```ruby
azure_queue_service = Azure::QueueService.new
```

Használjon hello **create_queue()** metódus toocreate hello rendelkező várólista adta meg.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: A várólista üzenet beszúrása
a várólistán, használjon hello üzenet tooinsert **create_message()** metódus toocreate egy új üzenetet, és adja hozzá toohello várólista.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a>Útmutató: A következő üzenetet hello betekintés
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **betekintés\_messages()** metódust. Alapértelmezés szerint **betekintés\_messages()** lekéri egyetlen üzenetben. Azt is megadhatja, hogy hány üzenetek toopeek szeretné.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a>Útmutató: A következő üzenetet hello created
Eltávolíthatja a üzenetet az üzenetsorból két lépésben.

1. A hívás esetén **lista\_messages()**, a hibaüzenet hello tovább a sorhoz alapértelmezés szerint. Azt is megadhatja, hogy hány üzenetek tooget szeretné. hello származó üzenetek **lista\_messages()** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. Át a hello láthatósági időkorlátot másodpercben paraméterként.
2. toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **delete_message()**.

A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy amikor a kód sikertelen tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kaphat hello ugyanazon üzenet, majd próbálkozzon újra. A kód hívások **törlése\_message()** jobb gombbal az üdvözlő üzenet feldolgozása után.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Útmutató: Módosítsa az sorba állított üzenetek hello tartalma
Módosíthatja egy üzenet helyben hello várólista hello tartalmát. hello kódot használja hello **update_message()** metódus tooupdate üzenetet. hello metódus visszaadható hello pop hello várólista üzenet fogadása tartalmazó rekordot és UTC dátum időt jelző érték amikor üdvözlőüzenetére hello várólista látható lesz.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Útmutató: További beállítások üzenetmozgatót üzenetek
Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.

1. A kötegelt üzenet kérheti le.
2. Megadhat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.

hello alábbi példakód hello **lista\_messages()** metódus tooget 15 üzenetek egy hívásban. Majd kinyomtatja, és törli az egyes üzeneteket. Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a>How To: Get hello várólistájának hossza
Hello várólistában lévő üzenetek száma hello felmérését kérheti le. Hello **beolvasása\_várólista\_metadata()** metódus tesz hello várólista szolgáltatás tooreturn hello hozzávetőleges üzenet számán és a metaadatok hello várólista kapcsolatban.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Útmutató: A várólista törlése
toodelete várólista és köszönőüzenetei minden benne tárolt, hívás hello **törlése\_queue()** hello várólista-objektum metódust.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* A Microsoft hello [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából

Összehasonlítása a hello Azure Queue szolgáltatás az a cikk és az Azure Service Bus-üzenetsorok hello tárgyalt tárgyalt [hogyan toouse Service Bus-üzenetsorok](/develop/ruby/how-to-guides/service-bus-queues/) cikk című [Azure várólisták és a Service Bus-üzenetsorok - képest és Ellentétben](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
