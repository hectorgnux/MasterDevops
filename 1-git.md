Perfecto 😎 — te dejo todo el contenido en **formato Markdown (.md)**, listo para copiar y pegar directamente en GitHub (por ejemplo en un archivo `README.md` o en tu repositorio de aprendizaje DevOps).

---

````markdown
# 🧑‍💻 Guía Completa de GitHub para DevOps

Aprende todo lo necesario para usar **GitHub como herramienta DevOps profesional**, desde control de versiones hasta automatización, despliegues e infraestructura como código.

---

## 🧭 1. Conceptos Fundamentales de Git y GitHub

### 🔹 Git básico
- **Repositorio (repo):** carpeta donde se guarda el código versionado.  
- **Commit:** registro de cambios.  
- **Branch (rama):** línea de desarrollo independiente.  
- **Merge / Pull Request:** integrar ramas mediante revisión.  
- **Clone / Fork:** clonar = copiar repo; fork = crear copia remota.

### 🔹 Comandos esenciales

```bash
git init             # Crear un repo local
git clone URL        # Clonar un repo remoto
git add .            # Agregar cambios
git commit -m "mensaje"
git push origin main # Subir al repo remoto
git pull             # Traer cambios del remoto
git branch           # Ver ramas
git checkout -b nueva-rama
````

---

## 🧩 2. GitHub: Colaboración y Estructura

### 🔸 Issues

Usa **Issues** para reportar errores, planificar tareas o sugerir cambios.

### 🔸 Pull Requests (PR)

Propón cambios de código y permite revisarlos antes de integrarlos en la rama principal.

### 🔸 Projects / Boards

Crea tableros tipo **Kanban** para organizar tareas DevOps, bugs y automatizaciones.

### 🔸 Wiki

Documenta tu infraestructura o procesos en la Wiki del repositorio.

---

## ⚙️ 3. GitHub Actions (CI/CD)

**GitHub Actions** permite crear flujos automatizados (pipelines) que se ejecutan cuando ocurre un evento, como un push, un pull request o un deploy.

### 🔹 Ejemplo de Workflow

Archivo: `.github/workflows/deploy.yml`

```yaml
name: Deploy App
on:
  push:
    branches: [ main ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Clonar repo
        uses: actions/checkout@v3

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Instalar dependencias
        run: npm install

      - name: Ejecutar tests
        run: npm test

      - name: Desplegar
        run: npm run deploy
```

### 🔹 Conceptos clave

* **Jobs:** se ejecutan en **runners** (máquinas virtuales o self-hosted).
* **Secrets:** claves seguras (tokens, credenciales, etc.).
* **Triggers:** eventos que activan el workflow (`push`, `pull_request`, `schedule`, etc.).

---

## 🔐 4. Seguridad y DevSecOps

### 🔸 Secrets y Variables

Guarda credenciales en:

> `Settings → Secrets and variables → Actions`

Uso en un workflow:

```yaml
env:
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
```

### 🔸 Dependabot

Actualiza dependencias y detecta vulnerabilidades automáticamente.

### 🔸 Code Scanning

Integra análisis estático (CodeQL, SonarCloud, etc.) para mejorar la seguridad.

---

## ☁️ 5. Integración con Infraestructura (IaC)

**GitHub** se integra con herramientas de **Infraestructura como Código** (IaC):

| Herramienta           | Uso principal                                   |
| --------------------- | ----------------------------------------------- |
| **Terraform**         | Desplegar recursos en la nube (AWS, Azure, GCP) |
| **Ansible**           | Configurar servidores                           |
| **Docker**            | Contenedores y CI/CD                            |
| **Kubernetes / Helm** | Orquestación y despliegues automáticos          |

### 🔹 Ejemplo con Terraform

```yaml
name: Terraform Deploy
on: [push]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Instalar Terraform
        uses: hashicorp/setup-terraform@v3

      - name: Init & Apply
        run: |
          terraform init
          terraform apply -auto-approve
```

---

## 🧠 6. GitOps y Automatización Avanzada

El modelo **GitOps** consiste en que toda la configuración de infraestructura vive en Git.
Cada cambio (PR o merge) dispara un **despliegue automático**.

Ejemplo:

* Modificas `values.yaml` de un servicio en Kubernetes.
* GitHub Actions ejecuta un `helm upgrade` automáticamente.

---

## 📊 7. Métricas, Monitoreo y Auditoría

* **Audit Logs:** registra quién hizo qué en el repositorio.
* **GitHub Insights:** mide actividad, commits y contribuciones.
* **Integraciones externas:** Prometheus, Grafana, Datadog, CloudWatch.

---

## 👥 8. Roles, Organizaciones y Buenas Prácticas

* Usa **organizations** y **teams** para separar proyectos y permisos.
* Aplica **branch protection rules** en `main` o `prod`.
* Usa nombres consistentes:
  `feature/`, `fix/`, `infra/`, `hotfix/`.
* Requiere **code review** antes de cada merge.

---

## 💼 9. Competencias Clave para DevOps

| Área                 | Habilidad con GitHub        |
| -------------------- | --------------------------- |
| Control de versiones | Git, ramas, PRs, tags       |
| CI/CD                | Workflows y pipelines       |
| IaC                  | Terraform, Ansible, Docker  |
| Cloud                | Integración AWS, Azure, GCP |
| Seguridad            | Secrets, CodeQL, Dependabot |
| Colaboración         | Issues, Projects, Wiki      |

---

## 🚀 10. Ruta de Práctica Recomendada

1. **Crea tu primer repo** y sube un proyecto simple.
2. **Agrega una GitHub Action** que ejecute tests o un despliegue.
3. **Configura un secreto** para usar credenciales de AWS o Docker Hub.
4. **Despliega automáticamente** una app a EC2 o S3.
5. **Implementa Terraform** para que GitHub gestione tu infraestructura.

---

## 🏁 Próximos pasos

👉 Aprende sobre:

* **GitHub Environments** (para producción/staging)
* **GitHub Packages / Container Registry**
* **Self-hosted runners**
* **Integraciones con Jenkins, ArgoCD o AWS CodeDeploy**
