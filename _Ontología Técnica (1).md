

## ---

**🏗️ Capa 2: Ontología Técnica de la Zona de Knox (Knowledge Graph)**

Esta capa define cómo "piensa" el asistente. No solo almacena datos, sino que entiende que una mordida en el brazo (suceso) requiere una sierra (ítem) y nivel 2 de primeros auxilios (requisito) para ejecutar una amputación (procedimiento).

### **1\. Definición de Entidades Críticas (Nodos)**

Para que KnoxMentor sea un experto, su cerebro se organiza en estas clases:

* **Mecánica\_Hardcore**: Define sistemas como "Infección Zombie", "Evento Helicóptero" y "Corte de Suministros".  
* **Componente\_Mod**: Entidades específicas de los 151+ mods (ej: *Kit de Amputación* de 'The Only Cure', *Prensa de Recarga* de 'The Workshop').  
* **Estado\_Moodle**: Condiciones del jugador (Hambre, Pánico, Hemorragia, Cansancio) que alteran las probabilidades de éxito.  
* **Vector\_Acústico**: Relación entre calibres de armas de Brita y el radio de atracción de hordas en decibelios.

### **2\. Mapa de Relaciones Semánticas (Aristas)**

Aquí es donde reside la inteligencia del asistente:

* ARMA\_FUEGO \--**PRODUCE**\--\> RUIDO\_DB \--**ACTUALIZA**\--\> NIVEL\_ALERTA\_HORDA.  
* MORDIDA\_INFECTADA \--**REQUIERE**\--\> AMPUTACIÓN \--**DENTRO\_DE**\--\> VENTANA\_30\_MIN.  
* VEHÍCULO\_AUTOSAR \--**DEPENDE\_DE**\--\> ESTACIÓN\_DE\_CHATARRA \--**PARA**\--\> INSTALAR\_BLINDAJE.  
* DIARIO\_RECUPERACIÓN \--**PRESERVA**\--\> EXPERIENCIA\_XP \--**ANTE**\--\> MUERTE\_PERSONAJE.

## ---

**🛠️ Archivo de Configuración de Ontología: knox\_ontology\_v1.json**

Este archivo será cargado en el Ontology\_Mapper\_Engine para procesar tu base de conocimiento.

JSON

{  
  "ontology\_meta": {  
    "project": "KnoxMentor",  
    "version": "1.0",  
    "base\_game": "Build 41.78",  
    "mod\_count": 151  
  },  
  "entities": {  
    "Trauma\_System": {  
      "mods": \["The Only Cure"\],  
      "critical\_actions": \["Amputation", "Cauterization", "Prosthetic\_Fitting"\],  
      "fail\_conditions": \["Time\_Exceeded\_30m", "Septicemia"\]  
    },  
    "Ballistics\_System": {  
      "mods": \["Brita's Weapon Pack"\],  
      "attributes": \["Caliber", "Noise\_Radius\_Tiles", "Jam\_Chance"\],  
      "check\_logic": "Caliber\_To\_dB\_Mapping"  
    },  
    "Vehicle\_Fortification": {  
      "mods": \["AutoSAR", "Scrap Armor"\],  
      "requirements": \["The Workshop Station", "Metalworking\_Level"\]  
    }  
  },  
  "logic\_rules": \[  
    {  
      "trigger": "User\_Report\_Bite",  
      "action": "Immediate\_Check\_Location",  
      "branch": "If\_Limb\_Direct\_To\_Amputation\_Protocol"  
    },  
    {  
      "trigger": "Nightfall\_Day\_1",  
      "action": "Enforce\_Blackout\_Protocol",  
      "priority": "Extreme"  
    }  
  \]  
}

### ---

