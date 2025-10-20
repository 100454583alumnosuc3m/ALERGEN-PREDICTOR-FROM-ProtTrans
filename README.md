# Lector y Predictor de Proteínas 🧬

Este script de Python permite predecir la clase de una proteína (alérgeno o no alérgeno) utilizando un modelo SVM previamente entrenado. La entrada del script son los embeddings de las proteínas en formato HDF5 (`.h5`).

-----

## ⚙️ Requisitos

Asegúrate de tener las siguientes librerías instaladas en tu entorno de Python. Puedes instalarlas con `pip`:

```bash
pip install numpy pandas joblib h5py scikit-learn
```

-----

## 🚀 Uso

Para ejecutar el script, usa el siguiente comando en la terminal:

```bash
python PREDICTOR_PROTEINAS_H5.py <ruta_del_modelo.plk> <ruta_del_embedding.h5>
```

  - `<ruta_del_modelo.plk>`: La ruta al archivo del modelo SVM entrenado. Este archivo debe ser un objeto serializado con `joblib`.
  - `<ruta_del_embedding.h5>`: La ruta al archivo HDF5 (`.h5`) que contiene los embeddings de las proteínas a predecir.

### **Ejemplo**

Si tu modelo se llama `pesos_modelo.plk` y tus embeddings están en `dataset.h5`, el comando sería:

```bash
python PREDICTOR_PROTEINAS_H5.py pesos_modelo.plk dataset.h5
```

-----

## 📝 Formato de los Archivos

### **Modelo SVM (`.plk`)**

El script espera un modelo de clasificación binaria de `scikit-learn` serializado con `joblib`. Este modelo debe haber sido entrenado previamente en un conjunto de datos similar.

### **Archivo de Embeddings (`.h5`)**

El script está diseñado para leer un archivo HDF5 donde cada "dataset" dentro del archivo representa una proteína. El **nombre del dataset** se usa como el identificador de la proteína y el **contenido del dataset** es su vector de embedding (un array de NumPy).

**Ejemplo de estructura de archivo `.h5`:**

```
/
  - Dataset: 'NombreProteina1', Shape: (1024,), Dtype: float32
  - Dataset: 'NombreProteina2', Shape: (1024,), Dtype: float32
  - Dataset: 'NombreProteina3', Shape: (1024,), Dtype: float32
  ...
```

-----

## 📊 Salida

La salida del script se imprime directamente en la terminal, mostrando los resultados de la predicción para cada proteína en el archivo de entrada. Por cada proteína, verás la siguiente información:

  - **Nombre**: El identificador de la proteína.
  - **Predicción**: `1` si es un alérgeno, `0` si no lo es.
  - **Clase**: Descripción en texto de la predicción.
  - **Distancia al *boundary***: La distancia de la muestra al hiperplano de decisión del SVM. Un valor positivo indica la clase `1` y un valor negativo la clase `0`. Cuanto mayor sea el valor absoluto, mayor es la confianza del modelo.
  - **Probabilidad de No Alérgeno**: La probabilidad de que la proteína pertenezca a la clase `0`.
  - **Probabilidad de Alérgeno**: La probabilidad de que la proteína pertenezca a la clase `1`.