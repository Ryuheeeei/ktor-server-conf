# Ktor Server Conf

Sample projects of configuring Ktor server.

## Run with docker

### HOCON configuration

```shell
docker build -t ktor-server-config:hocon hocon/
docker run --env CONFIG_FILE_PATH=application_custom.conf ktor-server-config:hocon
```

Application works fine with following log:

```
2025-09-27 05:58:43.104 [main] INFO  Application - Autoreload is disabled because the development mode is off.
2025-09-27 05:58:43.163 [main] INFO  Application - Application started in 0.127 seconds.
2025-09-27 05:58:43.234 [DefaultDispatcher-worker-2] INFO  Application - Responding at http://0.0.0.0:8080
```

```shell
cd hocon
./gradlew buildJar
jar -tf build/libs/hocon-all.jar | grep "ConfigLoader" | head -20
```

```
META-INF/services/io.ktor.server.config.ConfigLoader
io/ktor/server/config/ConfigLoader$Companion.class
io/ktor/server/config/ConfigLoader.class
io/ktor/server/config/ConfigLoadersJvmKt.class
io/ktor/server/config/HoconConfigLoader.class
```

### HOCON configuration with YAML support

This includes `ktor-server-config-yaml` dependency to support YAML in HOCON files.

This works fine when configuration file is YAML format.

```shell
docker build -t ktor-server-config:hocon-yaml hocon-but-yaml-included/
docker run --env CONFIG_FILE_PATH=application_custom.yaml ktor-server-config:hocon-yaml
```

However, this fails when configuration file is HOCON format.

```shell
docker build -t ktor-server-config:hocon-yaml hocon-but-yaml-included/
docker run --env CONFIG_FILE_PATH=application_custom.conf ktor-server-config:hocon-yaml
```

This fails with following error:

```
Exception in thread "main" java.lang.IllegalArgumentException: Neither port nor sslPort specified. Use command line options -port/-sslPort or configure connectors in application.conf
        at io.ktor.server.engine.CommandLineKt.CommandLineConfig(CommandLine.kt:74)
        at io.ktor.server.netty.EngineMain.createServer(EngineMain.kt:39)
        at io.ktor.server.netty.EngineMain.main(EngineMain.kt:24)
```

```shell
cd hocon-but-yaml-included
./gradlew buildJar
jar -tf build/libs/hocon-but-yaml-included-all.jar | grep "ConfigLoader" | head -20
```

```
META-INF/services/io.ktor.server.config.ConfigLoader
io/ktor/server/config/yaml/YamlConfigLoader.class
io/ktor/server/config/ConfigLoader$Companion.class
io/ktor/server/config/ConfigLoader.class
io/ktor/server/config/ConfigLoadersJvmKt.class
io/ktor/server/config/HoconConfigLoader.class
```
