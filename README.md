# 3d-Secure-cloud-configserver

Servidor de configuración para el proyecto 3D-Secure - utilizando spring cloud config server

El objetivo es almacenar toda la configuración de forma tal que se encuentre versionada y respaldada por un repositorio git.

Permite almacenar propiedades segurizadas (claves y otras contraseñas)

## Ejemplo

Las aplicaciones configuradas como cloud config client consultarán de manera automática su configuración, si fuera necesario realizarlo de forma rest puede realizarse de la siguiente manera:

```
http://server:port/{application}/{profile}
```

o

```
http://server:port/{application}-{profile}.yml
```


Típicamente `{profile}` será el nombre de un ambiente (desa, qa, prod) o por defecto `default`
Por ejemplo, para solicitar la configuración del portal para el ambiente desa por línea de comandos, estas llamadas son equivalentes:

```console

curl localhost:8080/portal/desa

curl localhost:8080/portal-desa.yml

```

## Encripción

El servidor soporta el almacenado de propiedades encriptadas (usuarios, contraseñas, etc), por el momento mediante la utilización de una clave simétrica.
Para determinar el valor encriptado de una propiedad debe llamarse al servicio de encripción provisto por el servidor, de la siguiente manera:

```
curl localhost:8080/encrypt -d valor_a_encriptar
```

Este valor puede ser almacenado entonces, adicionándole el prefijo ```{cipher}``` en el archivo de configuracion correspondiente, por ejemplo:

```
clave.almacenada: '{cipher}7a794f1435686313d8ff77a5d126423340bdc6c2b77bed1017f5032a2ad2610c'
```

### IMPORTANTE

La clave simétrica puede variar entre los ambientes de Dev/QA vs PROD. Averiguarla antes de crear configs productivas

## Referencias

- [Referencia Oficial](http://cloud.spring.io/spring-cloud-config/single/spring-cloud-config.html)
- [Repositorio de configuración](https://github.com/redbeestudios/phe-config)
