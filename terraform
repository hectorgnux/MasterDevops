
---

````markdown
# ☁️ Guía Completa de Terraform para DevOps (Desde Cero)

Terraform es una herramienta de **Infraestructura como Código (IaC)** creada por **HashiCorp**.  
Te permite **definir, crear, modificar y destruir infraestructura** (como servidores, redes, contenedores o buckets) usando archivos de texto en formato **HCL (HashiCorp Configuration Language)**.

En lugar de crear recursos manualmente en la consola de AWS, Azure, GCP o Docker, Terraform lo hace **de forma automática y reproducible**.

---

## 🧠 1. ¿Qué es Infraestructura como Código?

La **Infraestructura como Código (IaC)** significa que toda tu infraestructura (servidores, redes, balanceadores, contenedores, etc.) se define en código.  
Esto tiene muchas ventajas:

- ✅ Repetible: puedes recrear entornos iguales (dev, test, prod).  
- 🔁 Versionable: usas **Git** para registrar cada cambio.  
- 🚀 Automatizable: puedes integrarlo con **GitHub Actions o Jenkins**.  
- 👥 Colaborativo: todos los ingenieros usan el mismo código base.

---

## ⚙️ 2. Instalación y Configuración de Terraform

### 🔸 Paso 1 — Instalar Terraform
Descarga desde el sitio oficial:  
👉 [https://developer.hashicorp.com/terraform/downloads](https://developer.hashicorp.com/terraform/downloads)

### 🔸 Paso 2 — Agregar al PATH
En Windows:
- Descomprime el `.zip` en `C:\Program Files\Terraform\`
- Agrega esa carpeta a las variables de entorno del sistema (`PATH`)

En Linux/macOS:
```bash
sudo apt install unzip
wget https://releases.hashicorp.com/terraform/1.8.0/terraform_1.8.0_linux_amd64.zip
unzip terraform_1.8.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/
````

### 🔸 Paso 3 — Verificar instalación

```bash
terraform -version
```

---

## 📁 3. Estructura Básica de un Proyecto Terraform

```
mi-proyecto-terraform/
├── main.tf          # Configuración principal (infraestructura)
├── variables.tf     # Variables (ej. tipo de instancia, región)
├── outputs.tf       # Resultados (ej. IP pública)
├── provider.tf      # Configuración del proveedor (AWS, Docker, etc.)
└── terraform.tfvars # Valores privados (claves, tokens)
```

---

## 🌍 4. Tu Primer Proveedor: AWS

Terraform se conecta a los servicios cloud mediante **proveedores**.
Ejemplo: si usas AWS, declaras el proveedor `aws`.

### 🔹 `provider.tf`

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  required_version = ">= 1.6.0"
}

provider "aws" {
  region     = "us-east-1"
  access_key = var.aws_access_key
  secret_key = var.aws_secret_key
}
```

### 🔹 `variables.tf`

```hcl
variable "aws_access_key" {
  description = "Clave de acceso de AWS"
}
variable "aws_secret_key" {
  description = "Clave secreta de AWS"
}
```

### 🔹 `terraform.tfvars`

```hcl
aws_access_key = "TU_ACCESS_KEY"
aws_secret_key = "TU_SECRET_KEY"
```

> ⚠️ **Importante:** nunca subas `terraform.tfvars` a GitHub (contiene tus credenciales).

---

## 🧩 5. Comandos Básicos de Terraform

| Comando              | Descripción                                       |
| -------------------- | ------------------------------------------------- |
| `terraform init`     | Inicializa el proyecto y descarga los proveedores |
| `terraform plan`     | Muestra qué recursos se crearán o cambiarán       |
| `terraform apply`    | Aplica los cambios y crea la infraestructura      |
| `terraform destroy`  | Elimina los recursos creados                      |
| `terraform fmt`      | Formatea el código                                |
| `terraform validate` | Valida la sintaxis y dependencias                 |

Ejemplo:

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

---

## 🏗️ 6. Primer Ejemplo: Crear un Servidor en AWS EC2

### 🔹 `main.tf`

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0" # Ubuntu 22.04 LTS
  instance_type = "t2.micro"

  tags = {
    Name = "Servidor-Web"
  }
}
```

### 🔹 Desplegar:

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

### 🔹 Obtener la IP:

```bash
terraform output
```

### 🔹 Eliminar los recursos:

```bash
terraform destroy -auto-approve
```

---

## 🔄 7. Variables y Outputs

Las **variables** permiten parametrizar tus configuraciones.
Los **outputs** devuelven valores útiles (como IPs, IDs, etc.).

### `variables.tf`

```hcl
variable "region" {
  description = "Región donde se desplegarán los recursos"
  default     = "us-east-1"
}
```

### `outputs.tf`

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

## 🧱 8. Módulos: Reutilizar Infraestructura

Los módulos son **bloques reutilizables** de Terraform (como funciones en programación).

### Estructura:

```
modules/
 └── ec2/
     ├── main.tf
     ├── variables.tf
     ├── outputs.tf
```

### Uso:

```hcl
module "servidor" {
  source        = "./modules/ec2"
  instance_type = "t2.micro"
}
```

---

## 📦 9. Gestión del Estado (State)

Terraform guarda un archivo `terraform.tfstate` con el estado actual de tu infraestructura.
Ese archivo es **muy importante**: no lo borres ni lo compartas.

Para trabajar en equipo, se guarda el estado en un **backend remoto** (por ejemplo en S3):

```hcl
terraform {
  backend "s3" {
    bucket  = "mi-bucket-terraform"
    key     = "infra/state.tfstate"
    region  = "us-east-1"
    encrypt = true
  }
}
```

---

## ⚡ 10. Integración con GitHub Actions (CI/CD)

Puedes automatizar la ejecución de Terraform usando **GitHub Actions**, por ejemplo para desplegar automáticamente en AWS al hacer un push a `main`.

`.github/workflows/terraform.yml`

```yaml
name: Terraform Pipeline
on:
  push:
    branches: [ main ]
jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
      - name: Terraform Init
        run: terraform init
      - name: Terraform Plan
        run: terraform plan
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main'
        run: terraform apply -auto-approve
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

---

## 🐳 11. Terraform + Docker

Terraform también puede crear **contenedores locales**.
Esto es útil para simular entornos antes de pasar a la nube.

### `main.tf`

```hcl
provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "web" {
  image = docker_image.nginx.latest
  name  = "web"
  ports {
    internal = 80
    external = 8080
  }
}
```

### Ejecución:

```bash
terraform init
terraform apply -auto-approve
```

Ahora podrás abrir `http://localhost:8080` y ver NGINX corriendo.

---

## ☸️ 12. Terraform + Kubernetes

Si ya tienes un clúster Kubernetes (por ejemplo EKS, GKE o local con minikube), puedes gestionarlo con Terraform.

```hcl
provider "kubernetes" {
  config_path = "~/.kube/config"
}

resource "kubernetes_namespace" "app" {
  metadata {
    name = "mi-aplicacion"
  }
}
```

---

## 🧰 13. Estructura Recomendada para Proyectos Reales

```
terraform/
├── modules/
│   ├── vpc/
│   ├── ec2/
│   └── s3/
├── environments/
│   ├── dev/
│   ├── qa/
│   └── prod/
├── main.tf
├── variables.tf
├── outputs.tf
└── backend.tf
```

Cada entorno (`dev`, `qa`, `prod`) puede tener su propio `tfvars` con parámetros distintos.

---

## 🧠 14. Conceptos Clave

| Concepto     | Explicación                                                      |
| ------------ | ---------------------------------------------------------------- |
| **Provider** | Plugin que conecta Terraform con un servicio (AWS, Docker, etc.) |
| **Resource** | Componente a crear (ej. instancia EC2, contenedor)               |
| **Module**   | Conjunto reutilizable de recursos                                |
| **State**    | Registro del estado actual de la infraestructura                 |
| **Plan**     | Previsualización de cambios antes de aplicar                     |
| **Apply**    | Aplica los cambios y crea/actualiza recursos                     |
| **Destroy**  | Elimina todo lo creado                                           |
| **Output**   | Valor devuelto tras la creación (ej. IP pública)                 |

---

## 🚀 15. Beneficios para DevOps

Terraform es una herramienta fundamental en el flujo DevOps porque te permite:

* **Automatizar despliegues** en la nube y contenedores.
* **Versionar infraestructura** igual que código.
* **Reducir errores humanos** al evitar configuraciones manuales.
* **Integrar CI/CD** con GitHub Actions.
* **Unificar entornos** (lo que tienes en dev es igual a prod).

---

## 📚 16. Recursos Recomendados

* [📘 Documentación oficial](https://developer.hashicorp.com/terraform/docs)
* [🎓 HashiCorp Learn](https://learn.hashicorp.com/terraform)
* [🐙 Ejemplos Terraform GitHub](https://github.com/hashicorp/terraform-guides)
* [🧰 Awesome Terraform](https://github.com/shuaibiyy/awesome-terraform)
* [🏆 Certificación Terraform Associate](https://developer.hashicorp.com/certifications/terraform-associate)

---

## 🏁 17. Conclusión

Terraform es la base de cualquier infraestructura moderna en la nube.
Si trabajas con **AWS, Docker o Kubernetes**, te permite construir y destruir entornos en segundos, sin depender de clicks manuales.

> 💡 Empieza creando pequeños laboratorios locales con Docker, luego pasa a AWS.
> Con práctica, entenderás la magia de “infraestructura declarativa”.

---

> 💼 *Autor: [Tu Nombre]*
> ☁️ *Guía práctica para aprender Terraform desde cero*
> 🧱 *Versión: 1.0*

```

---

