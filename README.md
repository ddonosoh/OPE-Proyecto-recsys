# Contextual Bandits – Evaluación off-policy

## Requisitos

- **Python 3.10.x** ([python.org](https://www.python.org/downloads/)). `obp` ([Open Bandit Pipeline](https://github.com/st-tech/zr-obp)) 0.5.7 no se instala en Python 3.11 o superior.
- Jupyter (VS Code o Jupyter Notebook) para abrir `setup.ipynb`.
- ~13 GB libres en disco para los datos descomprimidos.

## Estructura de directorios

Los notebooks asumen que se ejecutan desde la raíz del proyecto, con esta estructura:

```
Proyecto/
├── setup.ipynb
├── requirements.txt
├── README.md
└── Contenido/
    └── Datasets/
        ├── open_bandit_dataset/
        │   └── open_bandit_dataset/
        │       ├── bts/{all,men,women}/
        │       └── random/{all,men,women}/
        └── KuaiRand-Pure/
            └── data/
```

## Datos

| Dataset | Descarga | Descomprimido |
|---|---|---|
| Open Bandit Dataset | 413 MB | 11,75 GB |
| KuaiRand-Pure | 47 MB | 194 MB |

Desde `Contenido/Datasets/`:

```bash
curl -L -O https://research.zozo.com/data_release/open_bandit_dataset.zip
mkdir open_bandit_dataset && unzip open_bandit_dataset.zip -d open_bandit_dataset

curl -L -O https://zenodo.org/records/10439422/files/KuaiRand-Pure.tar.gz
tar -xzf KuaiRand-Pure.tar.gz
```

MD5 de `KuaiRand-Pure.tar.gz`: `0820331067a3784d9691136f772b35a7`.

## Instalación

0. Instalar Python 3.10.x (por ejemplo, `winget install --id Python.Python.3.10 --exact` en Windows).
1. Abrir `setup.ipynb` y correr la celda 1 con cualquier kernel. Crea `.venv/` con Python 3.10, instala `requirements.txt` y registra el kernel **Python 3.10 (recsys-obp)**.
2. Cambiar el kernel a **Python 3.10 (recsys-obp)**, reiniciarlo y correr el resto de las celdas.

Si Python 3.10 no se encuentra automáticamente, definir la variable de entorno `PYTHON310` con la ruta a su `python.exe`.

## Notas

- `matplotlib` queda fijo en 3.8.4: `obp` usa `seaborn` 0.11, que falla con `matplotlib` 3.9 o superior.
- `.venv/` y `Contenido/Datasets/` no están incluidos en el repositorio por razones prácticas.
