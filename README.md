# Taller Introductorio de Spark Scala

Organizado por el **Grupo Universitario de Informática (GUI)**.

* **Ponente:** Manuel Méndez Calvo ([manuel.men.calvo@gmail.com](mailto:manuel.men.calvo@gmail.com))
* **Fecha:** 26/11/2026
* **Duración estimada:** 1,5 horas
* **Modalidad:** Taller práctico (con una breve introducción teórica) con portátil propio

---

##  Descripción y público objetivo

Taller práctico enfocado al procesamiento de datos y **Big Data** utilizando **Apache Spark** con **Scala**. Combinaremos una breve base teórica sobre el funcionamiento de Spark con ejercicios prácticos directamente en código.

* **¿A quién va dirigido?**
  * Personas con **conocimientos intermedios de programación** o de **ingeniería de datos**.
  * Personas que hayan trabajado previamente con librerías o herramientas de análisis de datos como **Pandas**, **Polars**, **R** o similares.
  * Asistentes que hayan realizado el *Taller introductorio de Scala* (este es el siguiente paso lógico).

---

## Requisitos previos e instalación (Importante antes del taller)

>  **Importante:** La descarga de las dependencias de Spark puede tardar unos minutos dependiendo de la conexión. Por favor, **realiza todos estos pasos antes de acudir al taller** para aprovechar al máximo la sesión.

### 1. Instalar JDK (versión 11 o 17)

Comprueba en tu terminal si ya tienes instalado Java 11 o Java 17:

```bash
java -version
```

Si no dispones de una de estas versiones, instala **JDK 11** o **JDK 17** (por ejemplo, desde [Adoptium Temurin](https://adoptium.net/)).

### 2. Instalar IntelliJ IDEA

Descarga e instala IntelliJ IDEA desde la [web oficial de JetBrains](https://www.jetbrains.com/idea/download/).

> **Nota:** Tendrás acceso a una prueba gratuita de 30 días de todas las funcionalidades. Una vez terminado ese periodo, podrás seguir usándolo de manera gratuita con algunas funciones limitadas que no afectarán a nuestro trabajo.

### 3. Instalar el plugin de Scala

1. Abre **IntelliJ IDEA**.
2. Accede a **File > Settings** (o `IntelliJ IDEA > Settings` en macOS).
3. Ve a la sección **Plugins**, busca **"Scala"** en el *Marketplace* e instálalo.
4. **Reinicia IntelliJ** para aplicar los cambios.

### 4. Crear el proyecto en IntelliJ usando SBT

Utilizaremos **SBT** para gestionar las dependencias de Apache Spark:

1. Haz clic en **New Project**.
2. Selecciona **Scala** en el panel izquierdo y asigna un nombre a tu proyecto.
3. En **Build System**, elige **SBT**.
4. En **JDK**, selecciona **11** o **17**.
5. En **Scala**, selecciona la versión **`2.12.18`** (versión óptima para compatibilidad con Spark 3.x).
6. Pulsa **Create**.

### 5. Configurar el archivo `build.sbt`

En el explorador de archivos del panel izquierdo, busca y abre `build.sbt`. Modifica su contenido utilizando el código facilitado en el **Anexo 1**.

Una vez guardado, haz clic en el **icono flotante de recarga de SBT** que aparecerá en el editor para que IntelliJ descargue automáticamente todas las dependencias de Spark.

### 6. Comprobar la instalación ("Hello World" con Spark)

Para asegurarte de que Spark arranca sin errores en tu equipo:

1. En el panel izquierdo, navega hasta `src > main > scala`.
2. Crea un nuevo paquete (**New > Package**) llamado `com.taller`.
3. Dentro de `com.taller`, crea una nueva clase de Scala de tipo **Object** (**New > Scala Class > Object**) llamada `MainSpark`.
4. Copia y pega el código de prueba del **Anexo 2** y ejecútalo.

Si ves por consola el mensaje de confirmación junto con una tabla de 3 filas (`Ana`, `Carlos` y `Elena`), ¡ya lo tienes todo listo!

---

## Anexos

### Anexo 1 (`build.sbt`)

```scala
name := "taller-spark-scala"
version := "0.1"
scalaVersion := "2.12.18"

val sparkVersion = "3.5.0" // Valdría cualquier otra a partir de la 3. Lo dejo a vuestra elección

libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion
)
```

### Anexo 2 (`MainSpark.scala`)

```scala
package com.taller

import org.apache.spark.sql.SparkSession

object MainSpark {
  def main(args: Array[String]): Unit = {
    // Crear una sesión de Spark ejecutándose en local
    val spark = SparkSession.builder()
      .appName("Test Spark Taller")
      .master("local[*]")
      .getOrCreate()

    println("Hello World")
    println(" ¡SPARK SE HA INICIALIZADO CORRECTAMENTE! ")
    println(s" Versión de Spark: ${spark.version}")
    println("========================================")

    // Pequeña prueba creando un DataFrame con datos dummy
    val datos = Seq(("Ana", 28), ("Carlos", 34), ("Elena", 45))
    import spark.implicits._
    val df = datos.toDF("Nombre", "Edad")

    df.show()

    spark.stop()
  }
}
```

---

### NOTA IMPORTANTE.

Si da problemas el JDK 17, hacer lo siguiente:

Ir al menú de run -> Current File -> Run With Parameters -> ModifyOptions -> add VM Options 

Y pondremos lo siguiente --add-exports=java.base/sun.nio.ch=ALL-UNNAMED --add-opens=java.base/java.nio=ALL-UNNAMED

## Contacto y soporte

Si te surge algún error de configuración o compatibilidad antes de la sesión, contacta con **Manuel Méndez Calvo** en [manuel.men.calvo@gmail.com](mailto:manuel.men.calvo@gmail.com).
