# renfe-calibracion

Ethical reverse engineering aplicado a la simulación ferroviaria. Análisis de la física de adherencia, frenado y tracción de 11 series de locomotoras RENFE mediante pruebas experimentales.

# Proyecto de Calibración Paramétrica de Locomotoras Diésel para Open Rails

**Autor:** Francisco Gabriel Murillo Roldán

**Titulación:** Ingeniero de Minas

**Metodología:** Método Teluroncio (calibración empírica mediante pruebas estandarizadas)

## 📌 Resumen del Proyecto

Este repositorio documenta un proyecto de **ethical reverse engineering** aplicado al simulador ferroviario de código abierto **Open Rails**.

Se ha desarrollado un método de calibración propio (el "Método Teluroncio") para ajustar la física de adherencia, frenado y tracción de locomotoras diésel, utilizando **documentación técnica auténtica** (curvas de esfuerzo tractor, manuales de conducción, fichas técnicas de RENFE) y un riguroso **plan de pruebas empíricas** estandarizado en rampa del 2% con 600 toneladas.

Como resultado, se han calibrado **11 series de locomotoras** (ALCO y General Motors), abarcando distintas generaciones tecnológicas desde los años 60 hasta los 2000.

> ⚠️ **Importante**: Todo el análisis de archivos de configuración (.eng, .inc) se ha realizado exclusivamente sobre archivos de parámetros en texto plano con el único propósito de corregir y mejorar la física de simulación. Este proceso no implica descompilación de software, vulneración de sistemas de protección ni acceso a código fuente cerrado. En este repositorio no se redistribuye ningún archivo original de terceros.

## 🚂 Locomotoras Calibradas

| Serie RENFE | Fabricante | Potencia al riel (kW) | Peso (t) | `C` (CK) | Velocidad (2%, 600 t) | Tecnología |
|--------------|--------------|-----------------|------------|----------|-------------------|------------|
| 1300 / 313 | ALCO | 900 | 84 | 0.102 | 31 km/h | DC clásica |
| 1600 / 316 | ALCO | 1120 | 109.5 | 0.15 | 21 km/h | DC clásica |
| 1800 / 318 | ALCO | 1200 | 96 | 0.122 | 42 km/h | DC clásica |
| 2100 / 321 | ALCO | 1330 | 102 | 0.102 | 31 km/h | DC clásica |
| 319.0 | GM | 1342 | 111 | 0.102 | 44.5 km/h | AC/DC, sin control de patinaje |
| 319.2 | GM / MACOSA | 1395 | 110 | 0.155 | 28 km/h | AC/DC, sin control de patinaje |
| 319.3 | GM / MEINFESA | 1512 | 110 | 0.165 | 31 km/h | AC/DC, sin control de patinaje |
| 319.4 | GM / MEINFESA | 1399 | 116 | 0.18 | 28 km/h | AC/DC, **con control de patinaje** |
| 333.0 | GM / NOHAB | 1875 | 120 | 0.18 | 40 km/h | **Alta potencia** |
| 333.3 | Vossloh / Alstom | 2237 | 120 | 0.16 | 40 km/h | **Alta potencia, mercancías** |
| 333.4 | Vossloh / Alstom | 2237 | 120 | 0.16 | 40 km/h | **Alta potencia, viajeros** |

## 🎯 Logros Técnicos Clave

- **Desarrollo de un método de calibración propio**: basado en pruebas estandarizadas y validación empírica de resultados.
- **Ethical reverse engineering de parámetros de adherencia**: identificación del papel del parámetro `C` como "interruptor" de sensibilidad climática y tecnología de control de patinaje.
- **Calibración de frenos**: determinación del umbral de `ORTSMaxBrakeShoeForce` en 20 kN por zapata, validado empíricamente para prevenir el bloqueo de ruedas.
- **Análisis de logs y depuración**: uso del log de Open Rails como herramienta de diagnóstico para corregir inestabilidades de frenado y tracción.
- **Diseño de configuración modular**: centralización de parámetros en archivos `.inc`, mejorando la mantenibilidad y escalabilidad de la calibración.
- **Implementación de freno dinámico continuo**: control analógico totalmente independiente mediante `NumNotches ( 0 )`.

## 📁 Estructura del Repositorio

| Carpeta / Folder | Contenido |
|:---|:---|
| [`/docs/`](./docs/) | Documentación completa en español e inglés |
| [`/Fichas-Tecnicas/`](./Fichas-Tecnicas/) | Fichas por locomotora con parámetros finales |
| [`/Experimentos/`](./Experimentos/) | Informes detallados de experimentos |
| [`/Parametros-Clave/`](./Parametros-Clave/) | Tablas resumen comparativas |
| [`/OpenRails-Logs/`](./OpenRails-Logs/) | Logs de pruebas Antes/Después |
| [`/Curvas-Traccion/`](./Curvas-Traccion/) | Programas Maxima y CSVs de curvas propias |

## 📚 Fundamento Teórico

El Método Teluroncio se inspira en la modelización de la adherencia rueda-carril desarrollada en la literatura científica. En particular, el trabajo de Yuan et al. (2021) proporciona una revisión exhaustiva de los modelos de fricción (Coulomb, racional, exponencial) aplicados al cálculo de la adherencia, incluyendo el modelo racional de Bochet del que deriva el **algoritmo de Curtius-Kniffler** — el modelo de adherencia implementado en Open Rails.

Esta revisión valida la elección de un modelo de fricción dependiente de la velocidad (frente al modelo de Coulomb de fricción constante) para simular de forma realista el comportamiento de la adherencia en vehículos ferroviarios. El enfoque empírico del Método Teluroncio —pruebas estandarizadas en rampa, ajuste iterativo de los parámetros `A`, `B`, `C` y validación climática— es la aplicación práctica de estos principios teóricos al entorno de simulación de Open Rails.

**Referencia:**
> Yuan, Z., Wu, M., Tian, C., Zhou, J., & Chen, C. (2021). *A Review on the Application of Friction Models in Wheel-Rail Adhesion Calculation*. Urban Rail Transit, 7(1), 1–11. DOI: [10.1007/s40864-021-00141-4](https://doi.org/10.1007/s40864-021-00141-4)

## 🛠️ Metodología (Método Teluroncio)

1.  **Extracción de datos**: digitalización de curvas de esfuerzo de tracción y frenado desde documentación técnica original.
2.  **Prueba de referencia**: locomotora + 600 toneladas en rampa del 2%, midiendo velocidad, esfuerzo y potencia en el HUD.
3.  **Ajuste de parámetros**: modificación iterativa de `A`, `B`, `C` en el modelo de adherencia de Curtius-Kniffler hasta alcanzar un comportamiento realista.
4.  **Validación climática**: repetición de pruebas en condiciones de lluvia (`D=0.7`) y nieve (`D=0.4`) para verificar la sensibilidad climática.
5.  **Calibración de frenos**:
    *   Experimentación con 5 tipos de zapatas (`P6`, `P10`, `Composite`, `Disc_Pads`, `User_Defined`).
    *   Selección de `High_Friction_Composite` como material óptimo para locomotoras modernas.
    *   Determinación del límite de 20 kN por zapata para evitar avisos y garantizar un frenado controlado.
6.  **Documentación**: generación de fichas técnicas completas y tablas de parámetros para cada locomotora.

## 📖 Contenido del Repositorio

| Archivo / Carpeta | Descripción |
| :--- | :--- |
| **`/docs/`** | Documentación completa en español (README_ES.md) e inglés (README_EN.md). |
| **`/Fichas-Tecnicas/`** | Fichas técnicas de 11 locomotoras (ALCO 313, 316, 318, 321; GM 319.0, 319.2, 319.3, 319.4, 333.3, 333.4). |
| **`/Experimentos/`** | Documentación de experimentos clave: prueba de zapatas de freno en 333.3, implementación de freno dinámico continuo, ajuste de adherencia en 319.4. |
| **`/Parametros-Clave/`** | Tablas resumen con valores finales de `MaxForce`, `MaxPower`, `C`, etc., por serie. |
| **`/Curvas-Traccion/`** | Programas en Maxima/WxMaxima y archivos CSV para generación de curvas de tracción propias. |
| **`/OpenRails-Logs/`** | Logs comentados que muestran la evolución desde configuraciones inestables hasta la calibración final. |

## ⚠️ Aviso de Propiedad Intelectual

Los archivos originales de las locomotoras referenciados en este análisis son propiedad de sus respectivos autores. Este repositorio **no contiene ni redistribuye ningún archivo original** (incluyendo `.eng`, `.inc`, `.s`, `.sd` o cualquier otro activo).

Todos los análisis, valores de parámetros, documentación técnica y datos experimentales aquí presentados son el resultado de mi propio trabajo personal de calibración y prueba, realizado sobre mis copias legítimas de los archivos para mi uso privado en el simulador Open Rails.

Este proyecto constituye **ethical reverse engineering** llevado a cabo exclusivamente con fines educativos y de corrección técnica. No se ha realizado descompilación de software, vulneración de sistemas de protección ni acceso a código fuente cerrado. No se distribuye ningún trabajo original. Por lo tanto, no se infringe ninguna licencia ni derecho de propiedad intelectual. El propósito de este repositorio es estrictamente educativo y demostrar competencias técnicas en calibración de simulación y análisis paramétrico.

Si usted es el propietario de algún activo original referenciado en este trabajo y tiene dudas sobre su mención, por favor contacte conmigo directamente.

## 📜 Licencia

El contenido de este repositorio (documentación, análisis, experimentos) está bajo la **Licencia MIT**.

El texto completo de la licencia está disponible en el archivo [`LICENSE`](LICENSE) de este repositorio.

## ✍️ Atribución

Los modelos 3D, texturas, sonidos y archivos de configuración originales de las locomotoras son obra de sus respectivos autores. Mi contribución consiste exclusivamente en la calibración de parámetros físicos basada en documentación técnica original de ALCO, General Motors, RENFE y Vossloh / Alstom, validada mediante pruebas empíricas y documentada en este repositorio.

## 🙏 Agradecimientos

Este proyecto ha sido posible gracias a:

- La documentación técnica original de ALCO, General Motors, RENFE y Vossloh / Alstom.
- La comunidad de Open Rails (foros Elvas Tower y Coals to Newcastle).
- Herramientas asistidas por IA para soporte técnico y estructuración de documentación.
- La plataforma GitHub para la gestión y difusión del repositorio.

- ---

## 👤 Sobre el Autor 

**Francisco Gabriel Murillo Roldán**
Ingeniero de Minas 

Especializado en calibración paramétrica de material rodante para simulación ferroviaria, análisis de adherencia rueda-carril y metodología de pruebas empíricas aplicadas a Open Rails.

📧 **[tu-email@ejemplo.com](mailto:tu-email@ejemplo.com)**
🔗 **[linkedin.com/in/tu-perfil](https://linkedin.com/in/tu-perfil)**

---

*Última actualización: Mayo 2026*
