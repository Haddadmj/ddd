# Pure APIC Commands

## Login / Logout Command

```shell
apic login --server {server} --username {username} --password {password} --realm {realm}

apic logout --server {server}
```

### To List Realms

```shell
apic identity-providers:list --scope provider --server {server} --fields name,title
```

## Commands On Catalog

### List App of Catalog

```shell
apic apps:list --org {org} --server {server} --catalog {name} --consumer-org {consumer}
```

### List Gateway of Catalog

```
apic configured-gateway-services:list --scope catalog -c {catalog.name} -o {context.org} -s {context.server}
```

### List Consumer of Catalog

```
apic consumer-orgs:list --org {context.org} --server {context.server} --catalog {catalog.name}
```

### List Products of Catalog

```
apic products:list-all --org {context.org} --scope catalog --server {context.server} --catalog {catalog.name}
```

### Get Specific Product

```
apic products:get --catalog ${catalog} --org ${data.org} --server ${data.server} --scope catalog ${product.name}
```

### Create App in Catalog

```
apic apps:create --catalog ${catalog} --consumer-org ${data.consumerorg} --org ${data.org} --server ${data.server} ${input}
```

#### Note: input file for above command "Create App"

```
title: NeoLeap App
name: neoleap-app
client_id: a889537d2f7dfee84b0b36e32970e127
client_secret: neDX0CYe4OjvhaMYbRdycPsU0GilVUTP
```

## Global Policy Commands

### List Policies on catalog

```shell
apic global-policies:list-all --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog
```

### Get Policy Yaml File

```
apic global-policies:get --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog input-gp:1.0.0
```

## In order to deploy policy on catalog

### Create Policy

```shell
apic global-policies:create --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog {policy to be published.yaml}
```

### Get Policy URL

```
apic global-policies:get --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog {policy name you get when you create the policy} --fields url
```

Open “GlobalPolicy.yaml” file and rename "url" with "global_policy_url"

### Assign it as Input"Prehook" / Output"Posthook"

```shell
apic global-policy-prehooks:create --catalog {server} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog GlobalPolicy.yaml
```

```
apic global-policy-posthooks:create --catalog {server} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog GlobalPolicy.yaml
```

### To Delete Policy

```
apic global-policy-prehooks:delete --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog
```

```
apic global-policy-posthooks:delete --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog
```

```
apic global-policies:delete --catalog {catalog.name} --configured-gateway-service {gateway.name} --org {org.name} --server {server} --scope catalog {policy name you get when you list the policies}
```

# Helper File To Publish API on APIC

### Included with this refrence a file that help publish API. "apic-deploy2.bat" place it in the APIC folder of the workspace you need it in addition to download the CLI from the API Manager Server

### Modify environment-template.properties to include your username and password and rename it to environment.properties

## The Format to use it like this

```
apic-deploy2 {env.name} {catalog.name} {product.name}.yaml {consumer.name} {app.name} {plan}
```
