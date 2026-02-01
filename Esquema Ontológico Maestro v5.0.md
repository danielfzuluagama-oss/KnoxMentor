

# ---

**🕸️ ESQUEMA ONTOLÓGICO MAESTRO: CEREBRO DE KNOXMENTOR (v5.0)**

**META-DATOS DEL ARTEFACTO**

* **ID del Esquema**: KNX-ONTOLOGY-SCHEMA-v5  
* **Motor de Procesamiento**: Ontology\_Mapper\_Engine  
* **Complejidad**: Nivel Hardcore (151+ Mods integrados)  
* **Integridad Referencial**: Enlace directo a los 8 Artefactos Indexados.

## ---

**1\. 📐 DEFINICIÓN DE CLASES DE ENTIDAD (Entity Class Definition)**

Cada nodo en el cerebro de KnoxMentor pertenece a una de estas clases maestras, extraídas rigurosamente de tus investigaciones:

### **CLASE A: ESTADO BIOLÓGICO (Bio-State)**

* **Definición**: Condiciones fisiológicas del superviviente que requieren intervención inmediata.  
* **Fuente de Verdad**: \[CORE\_MECH\] (1\_Knox\_Knowledge\_Base.md).  
* **Sub-Tipos**:  
  * Trauma\_Infeccioso: (Mordidas, Rasguños). Propiedad crítica: ventana\_amputación (30 min).  
  * Estado\_Psicosomático: (Pánico, Estrés). Afecta directamente a la puntería y velocidad de recarga.

### **CLASE B: VECTOR BALÍSTICO (Ballistic-Vector)**

* **Definición**: Cualquier herramienta capaz de proyectar daño a distancia, regida por la acústica.  
* **Fuente de Verdad**: \[BALLISTICS\] (3\_Knox\_Quick\_Reference.md).  
* **Atributos Nucleares**:  
  * Calibre: (ej: .308, 9mm, 12g).  
  * Firma\_Sonora: Valor en casillas (Tiles) que define el radio de atracción de zombis.

### **CLASE C: INFRAESTRUCTURA TÉCNICA (Tech-Infrastructure)**

* **Definición**: Maquinaria y estaciones de trabajo necesarias para el crafteo avanzado de mods.  
* **Fuente de Verdad**: \[TECH\_DB\] (Tabla Mods PZ.pdf).  
* **Sub-Tipos**:  
  * Estación\_Taller: (The Workshop). Requisito para blindajes.  
  * Vehículo\_Modificado: (AutoSAR). Entidad compuesta por chasis \+ blindaje \+ accesorios.

### **CLASE D: EVENTO CRONOLÓGICO (Time-Event)**

* **Definición**: Sucesos inevitables disparados por el tiempo de servidor.  
* **Fuente de Verdad**: \[SURVIVAL\_PATH\] (4\_Plan\_Supervivencia\_Dia1.md).  
* **Ejemplos**: Corte\_Agua (Día 0-30), Helicóptero (Día 6-9), Invierno.

## ---

**2\. 🔗 MAPA DE RELACIONES CAUSALES (Relationship Logic)**

El asistente utilizará estas reglas lógicas para "pensar" antes de responder. Estas relaciones conectan los índices de investigación:

### **R1: RELACIÓN DE CONSECUENCIA ACÚSTICA**

* **Sintaxis**: (Usuario) \--\[DISPARA\]--\> (Arma) \--\[GENERA\]--\> (Ruido\_dB) \--\[ATRAE\]--\> (Horda\_Zombis)  
* **Lógica de Validación**:  
  * Consultar \[BALLISTICS\].  
  * SI Ruido\_dB \> Umbral\_Zona  
  * ENTONCES Resultado \= **MUERTE PROBABLE**.  
  * ACCIÓN: Emitir advertencia de "Alto al Fuego".

### **R2: RELACIÓN DE PROTOCOLO QUIRÚRGICO**

* **Sintaxis**: (Trauma\_Infeccioso) \--\[LOCALIZADO\_EN\]--\> (Extremidad) \--\[REQUIERE\]--\> (Intervención)  
* **Lógica de Validación**:  
  * Consultar \[CORE\_MECH\].  
  * SI Intervención \== "Amputación"  
  * VERIFICAR Inventario: ¿Tiene Sierra? ¿Tiene Torniquete?  
  * ACCIÓN: Guiar cirugía paso a paso.

### **R3: RELACIÓN DE DEPENDENCIA INDUSTRIAL**

* **Sintaxis**: (Receta\_Blindaje) \--\[NECESITA\]--\> (Estación\_Taller) \+ (Nivel\_Metalistería)  
* **Lógica de Validación**:  
  * Consultar \[TECH\_DB\].  
  * SI Estación\_Taller NO está presente en Base\_Usuario  
  * ENTONCES Crafteo \= **IMPOSIBLE**.  
  * ACCIÓN: Indicar ubicación de loot para encontrar la estación o sus materiales.

## ---

**3\. 💻 EL CÓDIGO DEL CEREBRO: knox\_ontology\_schema.json**

Este es el archivo técnico que cargaremos en el Ontology\_Mapper\_Engine. Observa cómo cada campo cita su fuente de autoridad indexada.

JSON

{  
  "ontology\_meta": {  
    "schema\_id": "KNX-ONTOLOGY-v5",  
    "description": "Ontología de Supervivencia Táctica para Project Zomboid Build 41+",  
    "authority\_sources": \["CORE\_MECH", "BALLISTICS", "TECH\_DB", "SURVIVAL\_PATH"\]  
  },  
  "knowledge\_graph\_structure": {  
    "nodes": {  
      "Biostate\_Trauma": {  
        "description": "Heridas críticas gestionadas por el mod The Only Cure",  
        "properties": \["bite\_location", "infection\_timer\_minutes", "is\_cauterized"\],  
        "validation\_source": "1\_Knox\_Knowledge\_Base.md"  
      },  
      "Weapon\_Platform": {  
        "description": "Armas de fuego del pack Brita con balística simulada",  
        "properties": \["caliber\_type", "noise\_radius\_tiles", "jam\_probability"\],  
        "validation\_source": "3\_Knox\_Quick\_Reference.md"  
      },  
      "Survival\_Milestone": {  
        "description": "Eventos críticos temporales del servidor",  
        "properties": \["day\_start", "day\_end", "threat\_level"\],  
        "validation\_source": "4\_Plan\_Supervivencia\_Dia1.md"  
      },  
      "Crafting\_Station": {  
        "description": "Infraestructura necesaria para crafteo de mods",  
        "properties": \["station\_id", "mod\_origin", "required\_fuel"\],  
        "validation\_source": "Tabla Mods PZ.pdf"  
      }  
    },  
    "edges": {  
      "MITIGATES\_RISK": {  
        "source": "Item\_Tool",  
        "target": "Biostate\_Trauma",  
        "logic": "IF tool \== 'Bone Saw' AND trauma \== 'Arm\_Bite' THEN amputation\_success \= TRUE",  
        "citation": "1\_Knox\_Knowledge\_Base.md"  
      },  
      "TRIGGERS\_RESPONSE": {  
        "source": "Survival\_Milestone",  
        "target": "Tactical\_Order",  
        "logic": "IF milestone \== 'Helicopter\_Event' THEN order \= 'STAY\_INDOORS'",  
        "citation": "4\_Plan\_Supervivencia\_Dia1.md"  
      }  
    }  
  },  
  "vector\_search\_config": {  
    "description": "Configuración de búsqueda híbrida para documentos indexados",  
    "indices": \[  
      { "name": "medical\_protocols", "source\_file": "1\_Knox\_Knowledge\_Base.md", "chunk\_strategy": "paragraph" },  
      { "name": "ballistics\_tables", "source\_file": "3\_Knox\_Quick\_Reference.md", "chunk\_strategy": "table\_row" },  
      { "name": "crafting\_ids", "source\_file": "Tabla Mods PZ.pdf", "chunk\_strategy": "line\_item" }  
    \]  
  }  
}

## ---

**4\. 🔍 VALIDACIÓN DE INTEGRIDAD**

Antes de sellar esta capa, el sistema realiza una auto-comprobación de consistencia:

1. **Check de Trauma**: ¿Existe una ruta lógica para tratar una mordida?  
   * *Resultado*: **SÍ**. Ruta: Biostate\_Trauma \-\> MITIGATES\_RISK \-\> Item\_Tool (Sierra).  
2. **Check de Balística**: ¿Está definido el costo de disparar un arma?  
   * *Resultado*: **SÍ**. Entidad Weapon\_Platform incluye propiedad noise\_radius\_tiles.  
3. **Check de Cronograma**: ¿Sabe el sistema qué hacer el día 6?  
   * *Resultado*: **SÍ**. Nodo Survival\_Milestone contiene lógica para Helicopter\_Event.

### ---

