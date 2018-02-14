Aquesta pràctica de programació es basa en les classes

El punt és crear una calculadora que faci sumes, restes multiplicacions i divisions entre dos nombres enters.

Aquesta calculadora té una memòria de 10 operacions, eliminant així la primera de totes al veure's la memòria plena

Calculator en un principi consta de 5 atributs, que s'han d'inicialitzar al constructor:

private char [] _operations;

Aqui es guarden tots els símbols de les operacions

private int [] _operands1;

Aqui es guarda el primer opreand

private int [] _operands2;

Aqui es guarda el segond operand

private int _numRecordedOperations

I aquí es manté un registre del numero d'operacions



A part, també consta amb x mètodes:

Calculator

Constructor

addOperation

Permet que l'usuari afegeixi una operació

deleteOperation

Permet que l'usuari elimini una operació en una posició donada

evalOperaton

Permet que l'usuari obtingui la solució d'una operació

printOperation

Permet imprimir una operació

printAllOperations
printHigherResultOperation
PrintHigherResultOperation

És com la d'abans però en aquesta et deixa escollir entre operacions concretes (+, -, *, /)

anyNegativeOperations

Permet que l'usuari sàpiga si alguna de les seves operacions ha donat resultat negatiu
