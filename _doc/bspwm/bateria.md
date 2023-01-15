---
title: Batería
sections:
  - Funcionamiento
  - Código
  - Automatización
---

### Funcionamiento

El script de batería (activado mediante un `cronjob`),

### Código

El código es bastante sencillo:

```bash
#!/bin/bash

# check if battery is charging
charging=$(acpi -a | grep on-line | wc -l)

# check if battery is low
# 15% is low
if [ $charging -eq 0 ]; then
 battery=$(acpi -b | grep -P -o '[0-9]+(?=%)')
 if [ $battery -le 15 ]; then
  notify-send --urgency critical "Batería baja" "Se recomienda cargar la batería. Queda un $battery %."
 fi
fi
```

### Automatización

Para hacer funcionar esto, empleo un `cronjob`, de forma que se ejecute el script cada 5 minutos.

```bash
# run script every 5 minutes
# 5 * 60 = 300 seconds
*/5 * * * * /path/to/script.sh
```
