[![CircleCI](https://circleci.com/gh/delphes/delphes.svg?style=shield)](https://circleci.com/gh/delphes/delphes) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3735069.svg)](https://doi.org/10.5281/zenodo.3735069)

# Delphes

README original en [README.original.md](./README.original.md)

## Instalación

Los problemas/errores que tuve durante la instalación fueron:

1. **Please make sure root-config can be found in path.** Hay que ejecutar (ver https://root.cern/install/):
```
$ source root/bin/thisroot.sh
```

## Ejecución de prueba

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

### Delphes tutorial organized in the context of FCC - 2014.

#### [Introducción](https://indico.cern.ch/event/315979/attachments/606727/834937/delphes_tutorial_intro.pdf)
Las diapositivas donde muestra como se "programa" un calorímetro o la propagación de partículas (25 en adelante) son útiles

#### [Taller](https://indico.cern.ch/event/315979/attachments/606727/834936/delphes_tutorial_hands-on.pdf)
* ROOT tree is a set of branches
* Branch contains a collection of analysis objects for every event. Information is stored in TClonesArray, which enables efficient storage an retrieval
* ExRootTreeReader – class simplifying access to ROOT tree data

##### Modules Information

* Modules communicate entirely via collections of universal objects (TObjArray of Candidate four-vector like objects).
* Any module can access TObjArrays produced by other modules using ImportArray method

##### Configuration

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

### [MG5+Delphes tutorial organised in Bologna - June 2016](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/TutorialBologna)

Las `cards` tienen la configuración del experimento a simular (CMS, ATLAS, etc).

Each card can be schematically divided in three parts:

* The "ExecutionPath" is where the simulation/reconstruction sequence of modules is defined
* The list of modules configurations. Cada módulo se configura en `module` de acuerdo a las variables que cada uno defina.
* The TreeWriter, where the user defines which objects he stores in the output tree.

You can find an explanation for most modules here:

https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Modules

Hay una explicación muy clara de como implementar un nuevo módulo con un ejemplo en la sección `Part IV - Write a new module`.

Add the following at the top of the card to run on 1k events only: `set MaxEvents 1000`

### [Delphes tutorial organised in Future Collider School in Pisa - September 2018](https://cp3.irmp.ucl.ac.be/projects/delphes/wiki/WorkBook/Tutorials/Pisa)

Tiene una breve descripción de cómo usar Pythia para simular eventos de algunas partículas en específico para luego usarlos como input en Delphes.

### The Belle II Physics Book (Sección 4. Belle II Simulation)  

Este capítulo contiene:
* Event Generators (Pythia). Simulan los procesos físicos que se dan en la colisión.
* Detector Simulation. Simulan los detectors del experimento.

Algunos análisis requieren generadores de eventos más específicos. Un software importante en Belle II es `basf2`.

## ¿Cómo implementar Belle II?
1. Simular la física de las colisiones usando algún simulador de eventos.
    * ¿Pythia funciona? Según el Physics Books, sí debería de funcionar.
    * Creo que aquí es donde vendría la consideración de que Belle II es asimétrico. Me parece que Pythia sí debería de poder simular este tipo de colisiones.
2. Revisar si los módulos ya existentes en Delphes sirven para simular los detectores de Belle II. Si no, hay que crear módulos propios.


