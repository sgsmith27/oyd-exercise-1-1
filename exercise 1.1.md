# Exercise 1.1 — What Will Terraform Do?

Curso: Optimizaciones y Desempeño — Cloud Deployment Automation  
Sesión: 1 — 23 de abril de 2026  

---

# Repositorio
oyd-exercise-1-1

# Task 1 — Inicialización y validación
## Comandos ejecutados
terraform init
terraform validate

## Salida de terraform validate
Success! The configuration is valid.

## Explicación
El comando terraform validate verifica la sintaxis y estructura de la configuración Terraform sin crear infraestructura. Este comando valida:
tipos de datos
referencias
argumentos
integridad general de la configuración

# Task 2 — Análisis del plan
## Comandos ejecutado
terraform plan

## Salida de terraform plan
Terraform will perform the following actions:
```text
#aws_s3_bucket.exercise will be created
  + +resource "aws_s3_bucket" "exercise" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "oyd-exercise-bucket-2026"
      + bucket_domain_name          = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + tags                        = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + tags_all                    = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

## Preguntas
1. ¿Cuántos recursos serán creados, modificados o destruidos?

Terraform planea:

Crear: 1 recurso
Modificar: 0 recursos
Destruir: 0 recursos

2. ¿Qué significa el símbolo +?

El símbolo + indica que Terraform planea crear ese recurso o atributo.

3. Ejemplo de (known after apply) y explicación

Ejemplo:

arn = (known after apply)

Terraform no puede conocer este valor antes de ejecutar apply porque AWS genera y devuelve el ARN únicamente cuando el recurso es creado exitosamente mediante la API.

# Task 3 — Predicción de cambios
## Bloque de tags actualizado
tags = {
  Environment = "dev"
  ManagedBy   = "terraform"
  Owner       = "SmithG"
}

## Sección diff del nuevo plan
```text
 #aws_s3_bucket.exercise will be created
  + resource "aws_s3_bucket" "exercise" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "oyd-exercise-bucket-2026"
      + bucket_domain_name          = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + tags                        = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
          + "Owner"       = "SmithG"
        }
      + tags_all                    = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
          + "Owner"       = "SmithG"
        }
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }
```

## Preguntas
1. ¿Terraform propuso destruir y recrear el bucket o actualizarlo en sitio?

Terraform no propuso destruir ni recrear el bucket. Continúa proponiendo crear el recurso porque la infraestructura todavía no existe y no se ha ejecutado terraform apply.

2. ¿Por qué es importante esta distinción?

Esta distinción es importante porque recrear un recurso puede provocar:

pérdida de datos
downtime
cambios de identificadores
ruptura de dependencias

Mientras que una actualización en sitio modifica únicamente atributos específicos manteniendo el recurso existente.

# Task 4 — Razonamiento sobre el state
## Comando Ejecutado:
ls -la

## Salida
total 24

drwxr-xr-x 4 smith smith 4096 Apr 30 01:42 .

drwxr-xr-x 3 smith smith 4096 Apr 30 01:31 ..

drwxr-xr-x 7 smith smith 4096 Apr 30 01:32 .git

drwxr-xr-x 3 smith smith 4096 Apr 30 01:36 .terraform

-rw-r--r-- 1 smith smith 1407 Apr 30 01:36 .terraform.lock.hcl

-rw-r--r-- 1 smith smith  382 Apr 30 01:41 main.tf

## Preguntas
1. ¿Existe el archivo .tfstate?

No, el archivo terraform.tfstate no existe.

2. ¿Qué indica la ausencia del archivo state?

La ausencia del archivo terraform.tfstate indica que terraform plan no crea infraestructura ni guarda estado. El archivo state únicamente se genera después de ejecutar terraform apply, cuando Terraform crea recursos reales y necesita registrar el estado actual de la infraestructura.
