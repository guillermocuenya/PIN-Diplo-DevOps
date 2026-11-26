# Proyecto Integrador DevOps - Kubernetes con Minikube

## 📌 Descripción
Este proyecto implementa un entorno Kubernetes en **Minikube** para el despliegue de aplicaciones y monitoreo utilizando **Prometheus** y **Grafana**. Además, permite la eliminación y limpieza del entorno con un script de cleanup.

## 🚀 Características Principales
- **Configuración Automática** de Kubernetes en Minikube.
- **Despliegue de NGINX** como una aplicación de prueba.
- **Instalación de Prometheus y Grafana** mediante Helm.
- **Almacenamiento Persistente** para Grafana con `PersistentVolumeClaim (PVC)`.
- **Scripts de Automatización** para instalación y cleanup.

---

## 📜 Requisitos
- **Sistema Operativo**: macOS o Linux.
- **Dependencias Instaladas**:
  - Docker
  - Minikube
  - Kubectl
  - Helm
  - Homebrew (solo macOS)

---

## 🔧 Instalación y Configuración
Para desplegar el entorno, ejecuta el siguiente script:

```bash
chmod +x scripts/setup_devops.sh
./scripts/setup_devops.sh
```

Este script:
1. **Verifica y configura Minikube**.
2. **Despliega NGINX** como aplicación de prueba.
3. **Instala Prometheus y Grafana con almacenamiento persistente**.
4. **Proporciona las URLs de acceso** a los servicios desplegados.

### 🔑 Acceso a los Servicios
Tras la ejecución del script, obtendrás las URLs de acceso a:
- **Grafana**: `http://<minikube-ip>:<port>`
- **Prometheus**: `http://<minikube-ip>:<port>`

Para obtener la contraseña de Grafana:
```bash
kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

---

## 🧹 Cleanup (Eliminación del Entorno)
Si deseas **eliminar todos los recursos y reiniciar el entorno**, ejecuta:

```bash
chmod +x scripts/cleanup_devops.sh
./scripts/cleanup_devops.sh
```

Este script:
1. **Elimina Prometheus y Grafana**.
2. **Borra el namespace de monitoreo**.
3. **Elimina el despliegue de NGINX**.
4. **Detiene y elimina Minikube**.

---

## 📊 Persistencia de Datos en Grafana
Grafana ha sido configurado con un `PersistentVolumeClaim (PVC)` para evitar la pérdida de dashboards tras reinicios. Puedes verificarlo con:

```bash
kubectl get pvc -n grafana
```

Si el estado es `Bound`, significa que los datos se están almacenando correctamente.

---

## 🛠️ Troubleshooting
### 1️⃣ **No puedo acceder a Grafana o Prometheus**
Ejecuta:
```bash
kubectl get pods -n grafana
kubectl get pods -n monitoring
```
Si los pods están en `Pending` o `CrashLoopBackOff`, intenta:
```bash
kubectl delete pod -n grafana --all
kubectl delete pod -n monitoring --all
```

### 2️⃣ **Error `SVC_UNREACHABLE` al acceder a los servicios**
Cambia el servicio a `NodePort`:
```bash
kubectl patch svc prometheus-server -n monitoring -p '{"spec": {"type": "NodePort"}}'
kubectl patch svc grafana -n grafana -p '{"spec": {"type": "NodePort"}}'
```

### 3️⃣ **El PVC no se ha creado correctamente**
Verifica los volúmenes con:
```bash
kubectl get pvc -n grafana
kubectl describe pvc -n grafana
```
Si el estado no es `Bound`, revisa los eventos y errores en `kubectl describe pvc grafana`.

---

## 📌 Conclusión
Este proyecto permite:
✅ Implementar Kubernetes en local con Minikube.
✅ Desplegar NGINX como aplicación de prueba.
✅ Instalar Prometheus y Grafana con persistencia.
✅ Visualizar métricas del clúster y sus recursos.
✅ Automatizar la instalación y eliminación del entorno.


