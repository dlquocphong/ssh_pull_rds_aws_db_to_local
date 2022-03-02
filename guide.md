**1. Laravel env:**
```
  REMOTE_DB_HOST=127.0.0.1  //set default host local
  REMOTE_DB_PORT=13306
  REMOTE_DB_DATABASE=
  REMOTE_DB_USERNAME=
  REMOTE_DB_PASSWORD=
```

**2. config("database.connections.remote_mysql):**
```
'remote_mysql' => [
            'driver' => 'mysql',
            'host' => env('REMOTE_DB_HOST', '127.0.0.1'),
            'port' => env('REMOTE_DB_PORT', '13306'),
            'database' => env('REMOTE_DB_DATABASE', ''),
            'username' => env('REMOTE_DB_USERNAME', ''),
            'password' => env('REMOTE_DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => false,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
```

**3. Ssh config path: ~/.ssh/config**
```
  Host projectName
    HostName 1.111.111.111
    User ec2-user
    Port 22
    IdentityFile ~/.ssh/key.pem
```

**4. Create ssh tunnel forward rds to local machine**
- Ping Id : ping abc.xyz.ap-northeast-1.rds.amazonaws.com
- =>> Ex: 11.11.11.11
- Run Command
```
  ssh projectName -N -L 13306:11.11.11.11:3306 -v  -v
```
- With parameter
    + projectName: Name connect setting in ~./ssh/config 
    + 13306: forward port in local
    + 11.11.11.11: ip rds
- Note: every time it runs it will connect

**5. Run server laravel**
```
  + env -> change: DB_CONNECTION=remote_mysql
  + php artisan serve
```
