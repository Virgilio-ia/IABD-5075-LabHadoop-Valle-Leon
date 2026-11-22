# Despliegue de un Clúster Multi-Nodo Apache Hadoop con Docker

Práctica de laboratorio del **Curso de Especialización en Inteligencia Artificial y Big Data** (Módulo 5074/5075 · RA 2, Real Decreto 279/2021).

## Arquitectura Desplegada

Topología contenerizada mediante **Docker Compose** sobre la red puente aislada `hadoop-net` empleando la imagen oficial `apache/hadoop:3`:

| Servicio | Contenedor | Rol en el clúster | Puerto expuesto |
| :--- | :--- | :--- | :--- |
| **NameNode** | `namenode` | Maestro de metadatos y espacio de nombres HDFS | `9870` (Web UI) |
| **DataNode 1–3** | `datanode1`, `datanode2`, `datanode3` | Almacenamiento físico distribuido de bloques | Interno (`9866`) |
| **ResourceManager** | `resourcemanager` | Planificador global de recursos del clúster YARN | `8088` (Web UI) |
| **NodeManager** | `nodemanager` | Ejecutor de contenedores de cómputo MapReduce | Interno |

### Parámetros clave de configuración (`docker/config`)
- **Tamaño de bloque HDFS (`dfs.blocksize`):** `67108864` bytes (64 MiB).
- **Factor de replicación (`dfs.replication`):** `2`.
- **Gestión de memoria YARN:** `2048 MB` máximos por *NodeManager* con contenedores de `512 MB` para *ApplicationMaster*, *Map* y *Reduce* (`minimum-allocation-mb=256`), evitando bloqueos por falta de memoria en entornos locales.

## Estructura del Repositorio

```text
IABD-5075-LabHadoop-Valle-Leon/
├── .gitignore
├── README.md
├── docker/
│   ├── config
│   └── docker-compose.yml
├── evidencias/
│   ├── 01_namenode_ui_9870.png
│   ├── 02_hdfs_fsck_bloques.png
│   ├── 03_simulacion_fallo_datanode2.png
│   └── 04_yarn_wordcount_8088.png
└── informe/
    └── Valle_Leon_InformeLabHadoop.pdf