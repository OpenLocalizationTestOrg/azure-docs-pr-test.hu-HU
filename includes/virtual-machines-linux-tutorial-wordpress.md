## <a name="install-wordpress"></a><span data-ttu-id="bcc0e-101">WordPress telepítése</span><span class="sxs-lookup"><span data-stu-id="bcc0e-101">Install WordPress</span></span>

<span data-ttu-id="bcc0e-102">Ha azt szeretné, tootry a Jegyzettömböt, a minta-alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-102">If you want tootry your stack, install a sample app.</span></span> <span data-ttu-id="bcc0e-103">Tegyük fel, a lépéseket követve hello telepítése hello nyílt forráskódú [WordPress](https://wordpress.org/) platform toocreate webhelyek és blogok.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-103">As an example, hello following steps install hello open source [WordPress](https://wordpress.org/) platform toocreate websites and blogs.</span></span> <span data-ttu-id="bcc0e-104">Egyéb munkaterhelések tootry tartalmaznak [Drupal](http://www.drupal.org) és [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="bcc0e-104">Other workloads tootry include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="bcc0e-105">A WordPress-telepítő a koncepció igazolása szolgál.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="bcc0e-106">További információk és éles telepítési beállításai: hello [WordPress dokumentáció](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="bcc0e-106">For more information and settings for production installation, see hello [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-hello-wordpress-package"></a><span data-ttu-id="bcc0e-107">Hello WordPress csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="bcc0e-107">Install hello WordPress package</span></span>

<span data-ttu-id="bcc0e-108">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bcc0e-108">Run hello following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="bcc0e-109">A WordPress konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bcc0e-109">Configure WordPress</span></span>

<span data-ttu-id="bcc0e-110">WordPress toouse MySQL és a PHP konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-110">Configure WordPress toouse MySQL and PHP.</span></span> <span data-ttu-id="bcc0e-111">Futtassa a következő parancs tooopen hello egy tetszőleges szövegszerkesztőben, és hozzon létre hello fájlt `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="bcc0e-111">Run hello following command tooopen a text editor of your choice and create hello file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="bcc0e-112">Másolás hello következő sorok toohello fájl, és az adatbázis-jelszót *yourPassword* (hagyja változatlanul más értékek).</span><span class="sxs-lookup"><span data-stu-id="bcc0e-112">Copy hello following lines toohello file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="bcc0e-113">Mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-113">Then save hello file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="bcc0e-114">Egy munkakönyvtárba, hozzon létre egy szövegfájlt `wordpress.sql` tooconfigure hello WordPress adatbázis:</span><span class="sxs-lookup"><span data-stu-id="bcc0e-114">In a working directory, create a text file `wordpress.sql` tooconfigure hello WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="bcc0e-115">Adja hozzá a következő parancsokat, és az adatbázis-jelszót hello *yourPassword* (hagyja változatlanul más értékek).</span><span class="sxs-lookup"><span data-stu-id="bcc0e-115">Add hello following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="bcc0e-116">Mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-116">Then save hello file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="bcc0e-117">Futtassa a következő parancs toocreate hello adatbázis hello:</span><span class="sxs-lookup"><span data-stu-id="bcc0e-117">Run hello following command toocreate hello database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="bcc0e-118">Hello parancs befejezése után törölje hello fájlt `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-118">After hello command completes, delete hello file `wordpress.sql`.</span></span>

<span data-ttu-id="bcc0e-119">Helyezze át a hello WordPress telepítése toohello web server dokumentumgyökér:</span><span class="sxs-lookup"><span data-stu-id="bcc0e-119">Move hello WordPress installation toohello web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="bcc0e-120">Most hello WordPress beállításának befejezése és közzététele hello platformon.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-120">Now you can complete hello WordPress setup and publish on hello platform.</span></span> <span data-ttu-id="bcc0e-121">Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-121">Open a browser and go too`http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="bcc0e-122">Helyettesítse be a virtuális gép hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-122">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="bcc0e-123">Az alábbihoz hasonló toothis kép.</span><span class="sxs-lookup"><span data-stu-id="bcc0e-123">It should look similar toothis image.</span></span>

![WordPress telepítése oldal](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)