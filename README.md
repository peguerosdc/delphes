[![CircleCI](https://circleci.com/gh/delphes/delphes.svg?style=shield)](https://circleci.com/gh/delphes/delphes) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3735069.svg)](https://doi.org/10.5281/zenodo.3735069)

# Delphes

README original en [README.original.md](./README.original.md)

## Instalación

Los problemas/errores que tuve durante la instalación fueron:

1. **Please make sure root-config can be found in path.** Hay que ejecutar (ver https://root.cern/install/):
```
$ source root/bin/thisroot.sh
```

## Ejecucion de prueba

Para ejecutar el ejemplo `z_ee` (para descargarlo, ver el [README](./README.original.md) de Delphes):
```
$ ./DelphesSTDHEP cards/delphes_card_CMS.tcl delphes_output.root ../z_ee.hep
```
Y luego para ver algunas gráficas de este resultado, hay dos opciones. Una con C y otra con Python:
```
$ root -l examples/Example1.C'("delphes_output.root")'
$ python examples/Example1.py delphes_output.root
```
Como nota, el punto de entrada de este programa está en `/readers/DelphesSTDHEP.c`.

## Revisión de algunos recursos
En `cards` están los experimentos/detectores a escoger en el lenguaje TCL.

### [Workbook 1. Tutorials](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/RootTreeDescription)

#### Delphes tutorial organized in the context of FCC - 2014.

##### [Introducción](https://indico.cern.ch/event/315979/attachments/606727/834937/delphes_tutorial_intro.pdf)
Las diapositivas donde muestra como se "programa" un calorímetro o la propagación de partículas (25 en adelante) son útiles

##### [Taller](https://indico.cern.ch/event/315979/attachments/606727/834936/delphes_tutorial_hands-on.pdf)
* ROOT tree is a set of branches
* Branch contains a collection of analysis objects for every event. Information is stored in TClonesArray, which enables efficient storage an retrieval
* ExRootTreeReader – class simplifying access to ROOT tree data

###### Modules Information

* Modules communicate entirely via collections of universal objects (TObjArray of Candidate four-vector like objects).
* Any module can access TObjArrays produced by other modules using ImportArray method

###### Configuration

* Basado en TCL
* activate a module:
```
module ModuleClass ModuleName ModuleConfigurationBody
```
* define order of execution of various modules:
```
set ExecutionPath ListOfModuleNames
set ExecutionPath {
	ParticlePropagator
	TrackEfficiency
	TrackMomentumSmearing
	...
	TreeWriter
}
```
* hay un ejempo en la diapositiva 15
* All the output ROOT tree branches are configured in the TreeWriter module
* Para agregar un nuevo módulo, hay plantillas en `/modules/Example.h` y `/modules/Example.cc` y revisar la diapositiva 19

#### [MG5+Delphes tutorial organised in Bologna - June 2016](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/TutorialBologna)

Las `cards` tienen la configuración del experimento a simular (CMS, ATLAS, etc).

Each card can be schematically divided in three parts:

* The "ExecutionPath" is where the simulation/reconstruction sequence of modules is defined
* The list of modules configurations. Cada módulo se configura en `module` de acuerdo a las variables que cada uno defina.
* The TreeWriter, where the user defines which objects he stores in the output tree.

You can find an explanation for most modules here:

https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Modules

Hay una explicación muy clara de como implementar un nuevo módulo con un ejemplo en la sección `Part IV - Write a new module`.

Add the following at the top of the card to run on 1k events only: `set MaxEvents 1000`

#### [Delphes tutorial organised in Future Collider School in Pisa - September 2018](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Tutorials/Pisa)

Tiene una breve descripción de cómo usar Pythia para simular eventos de algunas partículas en específico para luego usarlos como input en Delphes.

### [Workbook 3. Root Tree Description](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/RootTreeDescription)

Explica las clases que se usan en el formato de salida en el que son guardados los datos producidos por Delphes para ser cargados con ROOT.

### [Workbook 4. Event Display](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/EventDisplay)

Explica como visualizar los datos producidos por Delphes. El comando que está ahí se puede ejecutar para visualizar los datos de la colisión de ejemplo en [Ejecución de prueba](#ejecucion-de-prueba).

### [Workbook 5. Module System](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/ModuleSystem)

Every Delphes module has a corresponding TFolder that is used to store TObjArrays produced by this module. Any Delphes module can access TObjArrays produced by other Delphes modules.

Da un ejemplo de cómo funcionan los módulos en Delphes. No dice mucho más que [el tutorial de Bologna](#MG5+Delphes-tutorial-organised-in-Bologna-June-2016).

### [Workbook 6. Candidates](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Candidate)

¿En qué parte de la simulación es usan estos Candidates?

### [Workbook 8. Candidates](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Arrays)

Los arreglos a los que pueden acceder los módulos. ¿Estos arreglos se van actualizando (cada módulo los va modificando) o son fijos en las condiciones iniciales? Según un diagrama generado por el comando de la sección Data Flow, parece que se van actualizando y cada módulo tiene acceso a todos.

### [Workbook . Data Flow](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/DataFlowDiagram)

Si no se instala graphviz, el programa no ejecuta:
```
sudo apt install graphviz
```
Podemos comparar el diagrama generado para CMS con alguna imagen [del flujo de partículas en el experimento](https://www.researchgate.net/profile/Mikael_Kuusela/publication/260003686/figure/fig1/AS:297130636922880@1447852872114/Illustration-of-the-detection-of-particles-at-the-CMS-experiment-Barney-2004-Each.png) para entender qué está pasando. ¿Cómo funcionaría esto para un experimento asimétrico?

### The Belle II Physics Book (Sección 4. Belle II Simulation)  

Este capítulo contiene:
* Event Generators (Pythia). Simulan los procesos físicos que se dan en la colisión.
* Detector Simulation. Simulan los detectors del experimento.

Algunos análisis requieren generadores de eventos más específicos. Un software importante en Belle II es `basf2`.


## ¿Cómo implementar Belle II?
1. Simular la física de las colisiones usando algún simulador de eventos.
    * ¿Pythia funciona? Según el Physics Books, sí debería de funcionar.
    * Creo que aquí es donde vendría la consideración de que Belle II es asimétrico.
      * Pythia ya es usado en Belle II para simular las colisiones
2. Revisar si los módulos ya existentes en Delphes sirven para simular los detectores de Belle II. Si no, hay que crear módulos propios.


