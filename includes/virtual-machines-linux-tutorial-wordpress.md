## <a name="install-wordpress"></a>WordPress telepítése

Ha azt szeretné, tootry a Jegyzettömböt, a minta-alkalmazások telepítése. Tegyük fel, a lépéseket követve hello telepítése hello nyílt forráskódú [WordPress](https://wordpress.org/) platform toocreate webhelyek és blogok. Egyéb munkaterhelések tootry tartalmaznak [Drupal](http://www.drupal.org) és [Moodle](https://moodle.org/). 

A WordPress-telepítő a koncepció igazolása szolgál. További információk és éles telepítési beállításai: hello [WordPress dokumentáció](https://codex.wordpress.org/Main_Page). 



### <a name="install-hello-wordpress-package"></a>Hello WordPress csomag telepítése

Futtassa a következő parancs hello:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>A WordPress konfigurálása

WordPress toouse MySQL és a PHP konfigurálásához. Futtassa a következő parancs tooopen hello egy tetszőleges szövegszerkesztőben, és hozzon létre hello fájlt `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Másolás hello következő sorok toohello fájl, és az adatbázis-jelszót *yourPassword* (hagyja változatlanul más értékek). Mentse a hello fájlt.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

Egy munkakönyvtárba, hozzon létre egy szövegfájlt `wordpress.sql` tooconfigure hello WordPress adatbázis: 

```bash
sudo sensible-editor wordpress.sql
```

Adja hozzá a következő parancsokat, és az adatbázis-jelszót hello *yourPassword* (hagyja változatlanul más értékek). Mentse a hello fájlt.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


Futtassa a következő parancs toocreate hello adatbázis hello:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Hello parancs befejezése után törölje hello fájlt `wordpress.sql`.

Helyezze át a hello WordPress telepítése toohello web server dokumentumgyökér:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Most hello WordPress beállításának befejezése és közzététele hello platformon. Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/wordpress`. Helyettesítse be a virtuális gép hello nyilvános IP-címét. Az alábbihoz hasonló toothis kép.

![WordPress telepítése oldal](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)