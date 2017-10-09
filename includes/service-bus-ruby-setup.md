## <a name="create-a-ruby-application"></a>Ruby-alkalmazás létrehozása
Útmutatásért lásd: [Ruby-alkalmazás létrehozása az Azure-on](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás tooUse Service Bus konfigurálása
a Service Bus toouse töltse le, és használja a hello Azure Ruby csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-rubygems-tooobtain-hello-package"></a>RubyGems tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).
2. Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.

### <a name="import-hello-package"></a>Hello csomag importálása
A kedvenc szövegszerkesztőjével használatával vegyen fel a következő toohello felső részén hello Ruby hello toouse tárolási profilokhoz fájlt:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>A Service Bus-kapcsolat beállítása
Használjon hello következő tooset hello értékek névtér neve hello kódot kulcs, a kulcs, a aláíró és a kiszolgáló:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Hello névtér érték toohello érték hello teljes URL-cím helyett létrehozott állítható be. Tegyük fel például, **"yourexamplenamespace"**, nem a "yourexamplenamespace.servicebus.windows.net".
