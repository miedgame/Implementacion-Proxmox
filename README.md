# Implementacion-Proxmox
Proyecto de diseño y despliegue de un entorno de pruebas, basado en Proxmox, que permita evaluar el rendimiento, la disponibilidad y la resiliencia de los servicios virtualizados
### 1. Arquitectura de la Solución
Se implementó un clúster de virtualización compuesto por **3 nodos físicos**, configurados para permitir la distribución de carga y la migración en vivo de servicios. La arquitectura se centra en eliminar puntos únicos de falla (SPOF) mediante:
* **Clustering de Nodos:** Gestión centralizada de recursos.
* **Almacenamiento CEPH:** Implementación de un pool de almacenamiento compartido (SSD) replicado entre los nodos.
* **Resiliencia:** Configuración de alta disponibilidad para máquinas virtuales (VMs).

### 2. Stack Tecnológico

| Capa | Tecnología | Rol |
| :--- | :--- | :--- |
| **Hipervisor** | Proxmox VE | Sistema base de virtualización |
| **Almacenamiento** | CEPH (Software Defined Storage) | Almacenamiento compartido replicado |
| **Hardware** | 3 Servidores Físicos | Nodos del clúster |
| **Gestión** | QEMU Agent | Optimización y control de VMs |

<p align="center">
  <img src="https://github.com/user-attachments/assets/TU_ID_IMAGEN_CLÚSTER" width="70%" alt="Vista del Clúster">
  <br>
  <em>Figura 1: Vista general del clúster con los tres nodos integrados y operativos.</em>
</p>

### 3. Especificaciones del Proyecto
El entorno fue diseñado bajo estándares de centro de datos para maximizar el rendimiento de los servicios distribuidos:
* **Nodos:** 3 servidores con Proxmox instalado.
* **Almacenamiento:** Creación del pool `ssdpool` distribuido para baja latencia.
* **Red:** Configuración de puentes virtuales (Linux Bridges) con soporte para VLANs y QEMU Agent activo.

<p align="center">
  <img src="https://github.com/user-attachments/assets/TU_ID_ANEXO_1" width="60%">
  <br>
  <em>Configuración de almacenamiento compartido "ssdpool" entre nodos.</em>
</p>

---

### 4. Objetivos del Proyecto
* **Alta Disponibilidad:** Configurar un clúster que permita la resiliencia de servicios ante fallos de hardware.
* **Escalabilidad:** Implementar almacenamiento **CEPH** para gestionar volúmenes de datos de forma distribuida.
* **Optimización de Recursos:** Distribuir máquinas virtuales estratégicamente entre los nodos para balancear la carga.
* **Estandarización:** Documentar procesos y manuales técnicos para la transferencia de conocimiento.

---

### 5. Resultados Obtenidos e Implementación Técnica

#### A. Despliegue de Nodos y Almacenamiento Distribuido
Se realizó la instalación limpia de Proxmox en los tres servidores, integrándolos en un solo pool de gestión. La implementación de CEPH permite que los datos existan simultáneamente en varios nodos.

| Configuración de Pool SSD | Selección de Imagen ISO |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/TU_ID_ANEXO_1" width="350"> | <img src="https://github.com/user-attachments/assets/TU_ID_ANEXO_2" width="350"> |
| *Creación de almacenamiento compartido.* | *Gestión centralizada de instaladores.* |

#### B. Aprovisionamiento de Máquinas Virtuales (VMs)
Se estandarizó el proceso de creación de VMs, optimizando el uso de CPU, memoria RAM con **ballooning** activo y optimización de disco mediante VirtIO.

| Configuración de CPU/RAM | Interfaz de Red (Bridge) |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/TU_ID_ANEXO_8" width="400"> | <img src="https://github.com/user-attachments/assets/TU_ID_ANEXO_10" width="400"> |
| *Asignación de núcleos y tipo de procesador.* | *Configuración de puente de red vmbr0.* |

#### C. Resiliencia y Monitoreo
Se validó la capacidad del clúster para mantener servicios activos y la visibilidad de los recursos de hardware desde la interfaz centralizada de Proxmox.

<p align="center">
  <img src="https://github.com/user-attachments/assets/TU_ID_IMAGEN_MONITOREO" width="80%">
  <br>
  <em>Gráficas de rendimiento y salud del clúster en tiempo real.</em>
</p>

---

### 6. Desafíos y Soluciones Técnicas

#### ⚠️ Incompatibilidad de Hardware (Kernel/OS)
* **Desafío:** Imposibilidad de instalar el sistema operativo en uno de los servidores debido a la obsolescencia del hardware, generando fallos críticos en el arranque del kernel.
* **Solución:** Evaluación de hardware y **sustitución del servidor físico** por uno compatible con las versiones actuales de Proxmox, asegurando la estabilidad del clúster.

#### ⚠️ Optimización de la Comunicación VM-Host
* **Desafío:** Falta de telemetría precisa y dificultades en el apagado controlado de las VMs desde el hipervisor.
* **Solución:** Implementación y activación de **QEMU Guest Agent** en todas las instancias virtuales, mejorando la gestión de memoria y la ejecución de comandos desde Proxmox.

---

### 7. Próximos Pasos (Roadmap)
* **Backup & Replication:** Instalación de un servidor de respaldo dedicado para logs y snapshots de VMs.
* **Seguridad Avanzada:** Implementación de **Wazuh Agent** en todos los nodos y VMs para fortalecer la detección de intrusiones en la infraestructura.
