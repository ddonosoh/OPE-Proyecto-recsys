# Contextual Bandits – Evaluación off-policy

## Requisitos

- **Python 3.10.x** ([python.org](https://www.python.org/downloads/)). `obp` ([Open Bandit Pipeline](https://github.com/st-tech/zr-obp)) 0.5.7 no se instala en Python 3.11 o superior.
- Jupyter (VS Code o Jupyter Notebook) para abrir `setup.ipynb`.
- ~15 GB libres en disco para los datos descomprimidos.

## Estructura de directorios

Los notebooks asumen que se ejecutan desde la raíz del proyecto, con esta estructura:

```
Proyecto/
├── setup.ipynb
├── requirements.txt
├── README.md
├── resultados/
│   └── anexo/
└── Contenido/
    └── Datasets/
        ├── open_bandit_dataset/
        │   └── open_bandit_dataset/
        │       ├── bts/{all,men,women}/
        │       └── random/{all,men,women}/
        ├── KuaiRand-Pure/
        │   └── data/
        └── KuaiRec/
            └── KuaiRec 2.0/
                └── data/
```

## Datos

| Dataset | Descarga | Descomprimido |
|---|---|---|
| Open Bandit Dataset | 413 MB | 11,75 GB |
| KuaiRand-Pure | 47 MB | 194 MB |
| KuaiRec 2.0 | 432 MB | 1,5 GB |

Desde `Contenido/Datasets/`:

```bash
curl -L -O https://research.zozo.com/data_release/open_bandit_dataset.zip
mkdir open_bandit_dataset && unzip open_bandit_dataset.zip -d open_bandit_dataset

curl -L -O https://zenodo.org/records/10439422/files/KuaiRand-Pure.tar.gz
tar -xzf KuaiRand-Pure.tar.gz

curl -L -O https://zenodo.org/records/18164998/files/KuaiRec.zip
mkdir KuaiRec && unzip KuaiRec.zip -d KuaiRec
```

MD5 de `KuaiRand-Pure.tar.gz`: `0820331067a3784d9691136f772b35a7`. MD5 de `KuaiRec.zip`: `261550d472c48eff4990fb13c0e5bcf7`.

## Instalación

0. Instalar Python 3.10.x (por ejemplo, `winget install --id Python.Python.3.10 --exact` en Windows).
1. Abrir `setup.ipynb` y correr la celda 1 con cualquier kernel. Crea `.venv/` con Python 3.10, instala `requirements.txt` y registra el kernel **Python 3.10 (recsys-obp)**.
2. Cambiar el kernel a **Python 3.10 (recsys-obp)**, reiniciarlo y correr el resto de las celdas.

Si Python 3.10 no se encuentra automáticamente, definir la variable de entorno `PYTHON310` con la ruta a su `python.exe`.

## Notebooks

| Notebook | Contenido |
|---|---|
| `setup.ipynb` | Entorno y verificación de los datos |
| `eda.ipynb` | Análisis exploratorio de OBD |
| `baselines.ipynb` | Random, Most Popular e IPWLearner en OBD evaluadas con IPS (primera entrega) |
| `ope.ipynb` | Réplica del protocolo de Saito et al. (2021) en OBD (*men*) |
| `KuaiRand-video-obp.ipynb` | Protocolo en KuaiRand-Pure con acción = video (versión principal) |
| `KuaiRand-obp-tag.ipynb` | Protocolo en KuaiRand-Pure con acción = primer tag (respaldo) |
| `KuaiRec-ope.ipynb` | Protocolo en KuaiRec con Random, Most Popular e IPWLearner |
| `dataset_sintetico.ipynb` | Construye el dataset sintético tipo OBD |
| `canibalizacion_inter.ipynb` | Canibalización en OBD (avance intermedio) |
| `canibalizacion_final.ipynb` | Canibalización en OBD y en el dataset sintético (versión final) |

Dependencias de orden: `ope.ipynb` genera `resultados/ope_men_n10000_tuning_por_semilla.csv`, que leen los notebooks de canibalización, y `dataset_sintetico.ipynb` genera `Contenido/Datasets/dataset_sintetico.npz`, que lee `canibalizacion_final.ipynb`, por lo que ambos deben correrse antes.

## Resultados guardados y recálculo

Los experimentos largos guardan sus resultados en `resultados/`, y al volver a ejecutar el notebook se leen desde ahí en lugar de recalcularse.

| Notebook | Archivo | Qué evita recalcular |
|---|---|---|
| `KuaiRec-ope.ipynb` | `resultados/checkpoint_kuairec_ipwlearner_top10.npz` | Entrenamiento del IPWLearner |
| `KuaiRec-ope.ipynb` | `resultados/kuairec_n10000.csv` | Las 200 semillas |
| `KuaiRand-video-obp.ipynb` | `resultados/kuairand_video_n10000.csv` | Las 200 semillas |
| `canibalizacion_final.ipynb` | `resultados/sintetico_estimadores_obp.csv` | Estimadores de obp en el dataset sintético |

En KuaiRec y KuaiRand video los resultados se guardan semilla por semilla, de modo que una ejecución interrumpida se retoma desde la última semilla completa y solo se calculan las que faltan. Para recalcular desde cero basta con borrar o renombrar el archivo correspondiente. Los resultados no cambian, ya que todas las semillas y modelos tienen `random_state` fijo, salvo diferencias en el último decimal entre máquinas.

Como referencia, en el equipo usado (Intel Core i5-13420H, 16 GB de RAM) el entrenamiento del IPWLearner tomó unos 40 minutos, las 200 semillas de KuaiRec unas 4 horas y las de KuaiRand video unas 16 horas. Cada ejecución usa entre 2 y 2,5 GB de RAM.

La salida de la celda del experimento en ambos notebooks corresponde a la corrida original. Al volver a ejecutarla se reemplaza por "200 semillas ya guardadas | faltan 0".

`resultados/anexo/` contiene la corrida de KuaiRec con `max_iter = 1000` en la regresión logística del IPWLearner, que no converge. Solo se usa para comparar con la versión principal (`max_iter = 10 000`).

## Notas

- `matplotlib` queda fijo en 3.8.4: `obp` usa `seaborn` 0.11, que falla con `matplotlib` 3.9 o superior.
- `.venv/` y `Contenido/Datasets/` no están incluidos en el repositorio por razones prácticas.
