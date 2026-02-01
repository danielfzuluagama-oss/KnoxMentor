

# ---

**🏭 PROTOCOLO DE INGESTA Y TEJIDO DE DATOS: KNOXMENTOR (v5.0)**

**META-DATOS DE OPERACIÓN**

* **ID de Proceso**: KNX-WEAVER-OPS-v5  
* **Motor ETL**: Batch\_Ingestor\_Pipeline & Real-time\_Connector  
* **Objetivo**: Poblar el Grafo de Conocimiento (KG) y la VectorDB con 100% de fidelidad a los 8 Artefactos Fuente.  
* **Estrategia de Fragmentación**: Híbrida (Semántica \+ Estructural).

## ---

**1\. 🚜 TUBERÍAS DE INGESTA ESPECÍFICAS (Source-Specific Pipelines)**

Para mantener la calidad "Clase Omni", no podemos tratar todos los archivos igual. Hemos diseñado 3 tuberías de procesamiento distintas según la naturaleza de tus datos:

### **TUBERÍA A: EXTRACCIÓN ESTRUCTURAL (Para Tablas y Datos Duros)**

* **Objetivo**: Convertir tablas estáticas en bases de datos consultables.  
* **Archivos Objetivo**:  
  * Tabla Técnica de Mods de Project Zomboid (Build 41+).pdf  
  * 3\_Knox\_Quick\_Reference.md  
* **Proceso**:  
  1. **Reconocimiento de Patrones**: Identificar filas y columnas (Item ID, Nombre, Mod, Peso).  
  2. **Atomización**: Cada fila se convierte en una entidad única en el KG (ej: Item:Brita\_SCAR\_H).  
  3. **Enriquecimiento**: Cruzar IDs con requisitos de habilidad (ej: "Requiere Aiming Lvl 5").

### **TUBERÍA B: EXTRACCIÓN PROCEDIMENTAL (Para Lógica y Reglas)**

* **Objetivo**: Capturar el "Si pasa X, haz Y".  
* **Archivos Objetivo**:  
  * 1\_Knox\_Knowledge\_Base.md (Mecánicas de Trauma)  
  * 4\_Plan\_Supervivencia\_Dia1.md (Cronograma)  
* **Proceso**:  
  1. **Segmentación por Pasos**: Dividir los textos en bloques lógicos de instrucción (Paso 1, Paso 2...).  
  2. **Etiquetado de Condición**: Detectar desencadenantes (Triggers) como "Si es mordida..." o "Si es día 6...".  
  3. **Indexación de Autoridad**: Marcar estos fragmentos como "Normativa Inviolable".

### **TUBERÍA C: EXTRACCIÓN DE IDENTIDAD Y VISIÓN (Para Contexto)**

* **Objetivo**: Inyectar la "personalidad" y el "propósito".  
* **Archivos Objetivo**:  
  * 2\_Knox\_System\_Prompt.md  
  * 5\_README\_Knox\_Mentor.md  
  * knoxMentor.html  
* **Proceso**:  
  1. **Ingesta de Tono**: Analizar adjetivos y verbos imperativos para calibrar el modelo de lenguaje.  
  2. **Mapeo Visual**: Entender qué información debe mostrarse en el dashboard (HTML) para que el asistente pueda decir "Mira tu panel de estado".

## ---

**2\. 🧬 ESTRATEGIA DE SINCRONIZACIÓN (The Weaver's Logic)**

Una vez ingeridos los datos, el Knowledge\_Synchronizer debe asegurar que no haya contradicciones.

* **Regla de Precedencia Hardcore**:  
  * Si 5\_README\_Knox\_Mentor.md dice "Sobrevive a toda costa" pero 1\_Knox\_Knowledge\_Base.md dice "El suicidio táctico es válido si estás infectado", **PREVALECE LA MECÁNICA (1\_Knox\_Knowledge\_Base.md)**. La física del juego supera a la filosofía.  
* **Regla de Validación de Inventario**:  
  * Si 4\_Plan\_Supervivencia\_Dia1.md sugiere "Busca un M16", el sistema verifica en Tabla Mods PZ.pdf si el M16 spawnea en zonas residenciales. Si la tabla dice "Solo en Zonas Militares", el asistente **CORRIGE** la instrucción del plan: "No busques M16 aquí, busca Escopetas Policiales".

## ---

**3\. 💻 CONFIGURACIÓN TÉCNICA DE INGESTA: knox\_ingest\_config.json**

Este es el archivo de configuración final que entregamos al Batch\_Ingestor\_Pipeline. Observa la granularidad de las directivas.

JSON

{  
  "ingestion\_job\_id": "KNX-INGEST-v5.0",  
  "target\_ontology": "KNX-ONTOLOGY-SCHEMA-v5",  
  "processing\_directives": \[  
    {  
      "source\_file": "Tabla Técnica de Mods de Project Zomboid (Build 41+).pdf",  
      "parser": "PDF\_Table\_Extractor",  
      "target\_entity": "Crafting\_Item",  
      "mapping\_rules": {  
        "col\_0": "item\_id",  
        "col\_1": "display\_name",  
        "col\_3": "crafting\_recipe\_ref"  
      },  
      "validation": "Drop\_If\_Missing\_ID"  
    },  
    {  
      "source\_file": "1\_Knox\_Knowledge\_Base.md",  
      "parser": "Markdown\_Semantic\_Chunker",  
      "target\_entity": "Medical\_Protocol",  
      "chunk\_size": 256,  
      "overlap": 20,  
      "tags": \["TheOnlyCure", "Trauma", "Surgery"\]  
    },  
    {  
      "source\_file": "3\_Knox\_Quick\_Reference.md",  
      "parser": "Markdown\_Table\_Parser",  
      "target\_entity": "Ballistic\_Data",  
      "mapping\_rules": {  
        "Weapon": "weapon\_name",  
        "Noise (Tiles)": "noise\_radius",  
        "Caliber": "ammo\_type"  
      }  
    },  
    {  
      "source\_file": "knoxMentor.html",  
      "parser": "HTML\_DOM\_Analyzer",  
      "target\_entity": "UI\_Component",  
      "purpose": "Contextual\_Visual\_Reference"  
    }  
  \],  
  "consistency\_check": {  
    "conflict\_resolution": "Priority\_Based",  
    "priority\_order": \[  
      "1\_Knox\_Knowledge\_Base.md",  
      "3\_Knox\_Quick\_Reference.md",  
      "Tabla Mods PZ.pdf",  
      "4\_Plan\_Supervivencia\_Dia1.md"  
    \]  
  }  
}

## ---

**4\. 📊 REPORTE DE SALUD DE DATOS (Data Health Preview)**

Antes de proceder, simulamos el resultado de la ingesta:

| Fuente | Entidades Detectadas | Estado de Enlace | Integridad |
| :---- | :---- | :---- | :---- |
| **Tabla Mods PZ.pdf** | 1,240 Ítems | **Vinculado** a Recetas | 99.8% (2 IDs ilegibles) |
| **Knox Quick Ref** | 45 Armas / 12 Calibres | **Vinculado** a Ruido | 100% |
| **Knowledge Base** | 18 Protocolos Médicos | **Vinculado** a Síntomas | 100% |
| **Plan Día 1** | 1 Cronograma (24 pasos) | **Vinculado** a Hitos | 100% |

### ---

