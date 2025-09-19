# Como usar este projeto

## 1️⃣ Gerar a imagem Docker `my-kc`

Execute o comando abaixo para construir a imagem a partir do `Dockerfile`:

```sh
docker build -t my-kc .
```

## 2️⃣ Gerar os certificados para o LDAP

Crie a pasta de certificados:

```sh
mkdir -p ldap/certs
```

Gere o certificado e a chave:

```sh
openssl req -new -x509 -days 365 -nodes \
	-out ldap/certs/ldap.crt \
	-keyout ldap/certs/ldap.key \
	-subj "/C=US/ST=Test/L=Test/O=Example/OU=IT/CN=ldap"
```

Copie o certificado para `ca.crt`:

```sh
cp ldap/certs/ldap.crt ldap/certs/ca.crt
```

## 3️⃣ Ajustar permissões dos certificados

```sh
chmod 644 ldap/certs/*.crt
chmod 600 ldap/certs/ldap.key
```

## 4️⃣ Reiniciar o LDAP

```sh
docker-compose down -v
docker-compose up -d ldap
```

## 5️⃣ Verificar se o LDAP está apresentando o certificado

```sh
openssl s_client -connect localhost:636 -showcerts
```

## 6️⃣ Converter `ca.crt` em um truststore para o Keycloak

Execute no host:

```sh
keytool -import -trustcacerts -noprompt \
	-alias ldapca \
	-file ldap/ca.crt \
	-keystore keycloak/truststore.jks \
	-storepass changeit
```

## 7️⃣ Subir o Keycloak

```sh
docker-compose up -d
```
