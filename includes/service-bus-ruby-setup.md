## <a name="create-a-ruby-application"></a><span data-ttu-id="6d672-101">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d672-101">Create a Ruby application</span></span>
<span data-ttu-id="6d672-102">Útmutatásért lásd: [Ruby-alkalmazás létrehozása az Azure-on](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="6d672-102">For instructions, see [Create a Ruby Application on Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="6d672-103">Az alkalmazás tooUse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d672-103">Configure Your application tooUse Service Bus</span></span>
<span data-ttu-id="6d672-104">a Service Bus toouse töltse le, és használja a hello Azure Ruby csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6d672-104">toouse Service Bus, download and use hello Azure Ruby package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="6d672-105">RubyGems tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="6d672-105">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="6d672-106">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="6d672-106">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="6d672-107">Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="6d672-107">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="6d672-108">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="6d672-108">Import hello package</span></span>
<span data-ttu-id="6d672-109">A kedvenc szövegszerkesztőjével használatával vegyen fel a következő toohello felső részén hello Ruby hello toouse tárolási profilokhoz fájlt:</span><span class="sxs-lookup"><span data-stu-id="6d672-109">Using your favorite text editor, add hello following toohello top of hello Ruby file in which you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="6d672-110">A Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="6d672-110">Set up a Service Bus connection</span></span>
<span data-ttu-id="6d672-111">Használjon hello következő tooset hello értékek névtér neve hello kódot kulcs, a kulcs, a aláíró és a kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="6d672-111">Use hello following code tooset hello values of namespace, name of hello key, key, signer and host:</span></span>

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

<span data-ttu-id="6d672-112">Hello névtér érték toohello érték hello teljes URL-cím helyett létrehozott állítható be.</span><span class="sxs-lookup"><span data-stu-id="6d672-112">Set hello namespace value toohello value you created rather than hello entire URL.</span></span> <span data-ttu-id="6d672-113">Tegyük fel például, **"yourexamplenamespace"**, nem a "yourexamplenamespace.servicebus.windows.net".</span><span class="sxs-lookup"><span data-stu-id="6d672-113">For example, use **"yourexamplenamespace"**, not "yourexamplenamespace.servicebus.windows.net".</span></span>
