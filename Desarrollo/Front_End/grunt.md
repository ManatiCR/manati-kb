# Grunt



###FAQ.

######1. ¿Qué hacer cuando se corrió grunt pero se cerró el proceso y al volver a tratar de correrlo tira error?

Nota, el error es:
Fatal error: *Port xxxxx is already in use by another process.*

Correr: 

*lsof | grep xxxxx*

Lo cual tirará una lista de procesos, normalmente, de node.

**Kill them all!**

El número al lado del nombre del proceso es el que debe usar. Eg:

Si el resultado del lsof es:

*node 666 user 20u IPv6 0x8247ae32e4a53925 0t0 TCP *:35729 (LISTEN)*

Entonces correr:

*kill 666*

---

