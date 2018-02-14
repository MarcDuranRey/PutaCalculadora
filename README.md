using System;

namespace Labdeliverable1
{
    public class Calculator
    {
        private const int MAX_MEMORY = 10;

        private char[] _operations;
        private int[] _operands1;
        private int[] _operands2;
        private int _numRecordedOperations;

        // E -> he afegit això, el problema es que no sé si es pot o no :shrug:
        // E -> _getRecordedOperations és simplement un getter per a que VerifyPosition del main funcioni
        public int _getRecordedOperations;

        public Calculator()
        {
            // E -> s'inicialitza tot
            _operations = new char[10];
            _operands1 = new int[10];
            _operands2 = new int[10];
            _numRecordedOperations = 1;
            _getRecordedOperations = _numRecordedOperations;
        }

        public void addOperation(char operation, int operand1, int operand2)
        {
            // E -> aqui faig que si el numero d'operacions guardades arriba a 10, es faci un delete
            if (_numRecordedOperations == MAX_MEMORY)
            {
                deleteOperation(1);
            }

            // E -> faig que els valors que entren per paràmetre s'incolguin a les arrays
            _operands1[_numRecordedOperations - 1] = operand1;
            _operands2[_numRecordedOperations - 1] = operand2;
            _operations[_numRecordedOperations - 1] = operation;

            // E -> aquí faig que cada cop que s'afegeix un valor a les arrays, es sumi 1 a _numRecordedOperations
            _numRecordedOperations++;

        }

        public void deleteOperation(int position)
        {
            // E -> per a que no dongui errors (no es surti fora dels limits) faig que si et donen una posició igual a la length (perque per molt que l'array vagi del 0 al 9, la length segueix
            //sent 10)...
            if(position == _operands1.Length)
            {
                // E -> simplement abuidi l'última posició i ya

                _operands1[position - 1] = 0;
                _operands2[position - 1] = 0;
                _operations[position - 1] = ' ';
            }
            else
            {
                // E -> aqui desplego l'array des de position (perque es comença a treballar des de position ex si )

                for (int i = position; i < _operations.Length && i < _numRecordedOperations - 1; i++)
                {
                    // E -> ...i faig es resti en una unitat la posició, i així es tira tot cap enrere

                    _operations[i - 1] = _operations[i];
                    _operands1[i - 1] = _operands1[i];
                    _operands2[i - 1] = _operands2[i];
                }

                // E -> i això es per que quedi null la última posició

                _operands1[_numRecordedOperations - 1] = 0;
                _operands2[_numRecordedOperations - 1] = 0;
                _operations[_numRecordedOperations - 1] = ' ';
            }

            // E -> i amb això faig que el número d'operacions guardades es redueixi, i així evites que elimini totes les operacions
            _numRecordedOperations--;
        }

        public int evalOperation(int operationPos)
        {
            // E -> Si es vol evaluar una posició que no té memòria guardada, s'ha de demanar que repeteixi (un while) -> fer servir funció VerifyPosition al Main
            int result = 0;
            

            if(_operations[operationPos] == '+')
            {
                result = _operands1[operationPos] + _operands2[operationPos];
            }
            else if(_operations[operationPos] == '-')
            {
                result = _operands1[operationPos] - _operands2[operationPos];
            }
            else if (_operations[operationPos] == '/')
            {
                result = _operands1[operationPos] / _operands2[operationPos];
            }
            else
            {
                result = _operands1[operationPos] * _operands2[operationPos];
            }

            return result;
        }

        public void printOperation(int operationPos)
        {
            // E -> simplement faig que escrigui la operació en la posició donada per l'usuari
            Console.Write(_operands1[operationPos] + " " + _operations[operationPos] + " " + _operands2[operationPos] + " = " + evalOperation(operationPos));
        }

        public void printAllOperations()
        {
            // E -> desplego tots els operands i els simbols...
            for (int i = 0; i <= _operands1.Length; i++)
            {
                // E -> i faig que s'imprimeixin un per un des de 0
                printOperation(i);
                Console.WriteLine();
            }
        }

        public void printHigherResultOperation()
        {
            // E -> aquí creo i incialitzo una variable temporal que acabará guardant el valor més alt, i una que guardarà la posició d'aquest valor
            int highest = 0, position = 0;

            // E -> primer desplego les arrays...

            for (int i = 0; i <= _numRecordedOperations; i++)
            {
                // E -> miro que si el resultat és més gran que la variable de control...
                if (evalOperation(i) > highest)
                {
                    // E -> ... aquest es guardi en ella...
                    highest = evalOperation(i);

                    // E -> ... i guardi la posició del resultat més alt...
                    position = i;
                }
            }

            // E -> ... i que finalment imprimeixi la evaluació d'aquesta posició
            printOperation(position);
        }

        public void printHigherResultOperation(char operationType)
        {
            // E -> creo una variable temporal i una de control
            int highest = 0, position = 0;

            // E -> primer desplego les arrays...
            for(int i = 0; i <= _numRecordedOperations; i++)
            {
                // E -> ... busco les que tinguin el tipus d'operació desitjat...
                if(_operations[i] == operationType)
                {
                    evalOperation(i);

                    if(evalOperation(i) > highest)
                    {
                        highest = evalOperation(i);

                        position = i;
                    }
                }
            }

            printOperation(position);
        }

        public bool anyNegativeOperation()
        {
            bool negativeOperations = false;

            // E -> primer creo una variable de control de si és negatiu i una que guarda les posicions
            int negative = 0, position = 0;

            // E -> desprès, creo un for...
            for (int i = 0; i <= _numRecordedOperations; i++)
            {
                // E -> ... i busco i algun dels resultats de eval operation dona negatiu...
                if(evalOperation(i) < negative)
                {
                    // E -> ... i si el troba, avisa...
                    negativeOperations = true;
                }
            }

            // E -> .. i si no, retorna false (ja que en un principi negativeOperations es false)

            return negativeOperations;
        }

    }


    class MainClass
    {
        public static void Main(string[] args)
        {
            // E -> aquí crido a la classe calculator per a poder fer-la servir en tot el main
            Calculator calculator = new Calculator();


            // E -> creo un switch per a desplegar totes les opcions del menú
            int menu;

            menu = Menu();

            switch (menu)
            {
                case 1:
                    // E -> opció en la que l'usuari pot afegir una operació a la calculadora
                    
                    // E -> faig que l'usuari afegeixi una operació mitjançant la funció AskAddOperation
                    if (AskAddOperation(out int operand1, out int operand2, out char operation))
                    {
                        calculator.addOperation(operation, operand1, operand2);
                    }
                    break;

                case 2:
                    // E -> opció en que l'usuari pot eliminar una operació d'una posició donada

                    // E -> creo una variable que determina la posició
                    int position = 0;

                    // E -> demano la posició de la operació que es vol eliminar
                    Console.Write("\n\nPlease, introduce the nº of the operation you want to delete (1 for the first, 2 for the second...): ");
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    bool pos = int.TryParse(Console.ReadLine(), out position);
                    Console.ResetColor();

                    while (!pos)
                    {
                        // E -> error, reask
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nERROR. That's not a valid option. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, introduce the nº of the operation you want to delete (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        pos = int.TryParse(Console.ReadLine(), out position);
                        Console.ResetColor();
                    }

                    // E -> i verifico la posició amb la funció VerifyPosition
                    while (!VerifyPosition(position, calculator))
                    {
                        // E -> si la posició està buida, error & try again
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nThere's no operation in that position. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, introduce the nº of the operation you want to delete (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        pos = int.TryParse(Console.ReadLine(), out position);
                        Console.ResetColor();

                    }

                    // E -> si la posició és correcta, es crida a deleteOperation per a que elimini la operació
                    calculator.deleteOperation(position);
                    break;

                case 3:

                    // E -> opció en la que l'usuari pot evaluar (obtindre el resultat de) una operació

                    // E -> primer creo una variable que estableix la posició de l'operació a evaluar
                    int evaluation;

                    // E -> li demano a l'usuari la posició de l'operació
                    Console.Write("\n\nPlease, select the position to evaluate (1 for the first, 2 for the second...): ");
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    bool eval = int.TryParse(Console.ReadLine(), out evaluation);
                    Console.ResetColor();

                    // E -> m'asseguro que el valor donat sigui correcte
                    while (!(eval))
                    {
                        // E -> error, reask
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nERROR. That's not a valid option. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, select the position to evaluate (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        eval = int.TryParse(Console.ReadLine(), out evaluation);
                        Console.ResetColor();
                    }

                    // E -> m'asseguro de que la posició no estigui buida mitjançant la funció VerifyPosition
                    while (!VerifyPosition(evaluation, calculator))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nThere's no operation in that position. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, select the position to evaluate (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        eval = int.TryParse(Console.ReadLine(), out evaluation);
                        Console.ResetColor();
                    }

                    // E -> i envio la posició a ser evaluada
                    calculator.evalOperation(evaluation);
                    break;

                case 4:

                    // E -> opció en la que l'usuari demana que s'imprimeixi una operació en una posició determinada

                    // E -> primer creo la variable que determinarà la posició
                    int positionPrint = 0;

                    // E -> demano a l'usuari que introdueixi la posició
                    Console.Write("\n\nPlease, select the operation to print (1 for the first, 2 for the second...): ");
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    bool posPrint = int.TryParse(Console.ReadLine(), out positionPrint);
                    Console.ResetColor();

                    // E -> m'asseguro que el valor donat sigui correcte
                    while(!posPrint)
                    {
                        // E -> error, reask
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nERROR. That's not a valid option. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, select the position to print (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        posPrint = int.TryParse(Console.ReadLine(), out evaluation);
                        Console.ResetColor();
                    }

                    // E -> m'asseguro de que la posició no estigui buida
                    while (!VerifyPosition(positionPrint, calculator))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nThere's no operation in that position. Try again");
                        Console.ResetColor();

                        Console.Write("\n\nPlease, select the position to print (1 for the first, 2 for the second...): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        int.TryParse(Console.ReadLine(), out positionPrint);
                        Console.ResetColor();
                    }

                    // E -> i faig que imprimeixi la operació
                    calculator.printOperation(positionPrint);
                    break;

                case 5:

                    // E -> opció que permet a l'usuari veure totes les operacions

                    // E -> comprobo que hi hagi operacions registrades (comprovant que com a mínim hi ha una operació registrada)...
                    if (!VerifyPosition(0, calculator))
                    {
                        // E -> ...Si no hi ha, s'avisa
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nSorry, there are no registered operations yet");
                        Console.ResetColor();
                    }
                    else
                    {
                        // E -> ...si hi ha, imprimeix
                        Console.WriteLine("\n\nHere they are: \n");
                        Console.ForegroundColor = ConsoleColor.DarkCyan;
                        calculator.printAllOperations();
                        Console.ResetColor();
                    }
                    break;

                case 6:

                    // E -> opció que permet a l'usuari veure l'operació amb el resultat més alt

                    // E -> comprovo que hi hagi operacions registrades (comprovant que com a mínim hi ha una operació regisrada)...
                    if (!VerifyPosition(0, calculator))
                    {
                        // E -> ...Si no hi ha, s'avisa
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("Sorry, there are no registered operations yet");
                        Console.ResetColor();
                    }
                    else
                    {
                        // E -> ...si hi ha, imprimeix
                        Console.WriteLine("Here it is: \n\n");
                        Console.ForegroundColor = ConsoleColor.DarkCyan;
                        Console.Write("\t\t");
                        calculator.printHigherResultOperation();
                        Console.ResetColor();
                    }
                    break;

                case 7:

                    // E -> opcioó que permet a l'usuari veure la operació amb el resultat més alt d'un signe concret (+, -, *, /)

                    // E -> primer declaro la variable que definira el signe
                    char sign;

                    // E -> desprès demana la operació...
                    Console.Write("Please, introduce the operation (+, -, *, /): ");
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    sign = Convert.ToChar(Console.ReadLine());
                    Console.ResetColor();


                    // E -> comprova que estigui bé...
                    while (!(sign == '+' || sign == '-' || sign == '*' || sign == '/'))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("\n\nThere's no operation with that sign. Try again");
                        Console.ResetColor();

                        Console.Write("Please, introduce the operation (+, -, *, /): ");
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        sign = Convert.ToChar(Console.ReadLine());
                        Console.ResetColor();
                    }

                    // E -> ... i després la printeja
                    Console.Write("\n\nHere it is: \n\n");
                    Console.ForegroundColor = ConsoleColor.DarkCyan;
                    Console.Write("\t\t");
                    calculator.printHigherResultOperation(sign);
                    Console.ResetColor();
                    break;

                case 8:

                    // E -> oopció que permet a l'usuari saber si alguna operació ha donat un resultat negatiu

                    if (!VerifyPosition(0, calculator))
                    {
                        // E -> ... Si no hi ha operacions, avisa...
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.Write("Sorry, there are no registered operations yet");
                        Console.ResetColor();
                    }
                    else
                    {
                        // E -> ... i si n'hi ha, fa saber si alguna és negativa
                        if (calculator.anyNegativeOperation())
                        {
                            Console.ForegroundColor = ConsoleColor.Green;
                            Console.Write("\n\nYes, there are operation with negative results");
                            Console.ResetColor();
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.Write("\n\nSorry, any of the operation has a negative result");
                            Console.ResetColor();
                        }
                    }
                    break;

                case 9:

                    // E -> opció que permet a l'usuari sortir del programa
                    Console.Clear();
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.Write("\n\nThanks for checking in!");
                    Console.ResetColor();
                    Console.ReadKey();
                    break;
            }

            // E -> amb això fem que el menu torni a aparèixer
            PressAnyKeyToContinue();
            Console.Clear();
            menu = Menu();
        }

        public static int Menu()
        {

            // E -> creo la funció menú per a que es mostri en pantala

            int option = 0;

            Console.Write("MENU");

            Console.Write("\n\n\n");

            Console.Write("\t1.- Add operation");
            Console.Write("\n\n\t2.- Delete operation");
            Console.Write("\n\n\t3.- Evaluate operation");
            Console.Write("\n\n\t4.- Print an operation at a given position");
            Console.Write("\n\n\t5.- Print all operations");
            Console.Write("\n\n\t6.- Print operation with highest result");
            Console.Write("\n\n\t7.- Print an specific (+, -, *, /) operation with highest result");
            Console.Write("\n\n\t8.- Any negative values?");
            Console.Write("\n\n\t9.- EXIT");

            Console.Write("\n\n\nPlease, select option: ");
            Console.ForegroundColor = ConsoleColor.Yellow;
            option = Convert.ToInt32(Console.ReadLine());
            Console.ResetColor();



            while (!(option >= 1 && option <= 9))
            {
                //error, reask

                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("\n\nIncorrect option. Correct options are 1, 2, 3, 4, 5, 6, 7, 8, 9");
                Console.ResetColor();
                Console.Clear();

                Console.Write("\n\n\nPlease, select option: ");
                Console.ForegroundColor = ConsoleColor.Yellow;
                option = Convert.ToInt32(Console.ReadLine());
                Console.ResetColor();
            }

            // E -> i aquí retorna el valor de l'opció escollida per l'usuari
            Console.Write("\n\n");
            PressAnyKeyToContinue();
            Console.Clear();
            return option;
        }

        public static bool AskAddOperation(out int operand1, out int operand2, out char operation)
        {
            // E -> aqui demana els elements de la operació a l'usuari
            Console.Write("\n\nPlease, add the first operand: ");
            Console.ForegroundColor = ConsoleColor.Yellow;
            bool op1 = int.TryParse(Console.ReadLine(), out operand1);
            Console.ResetColor();
            Console.Write("\n\nPlease, add the second operand: ");
            Console.ForegroundColor = ConsoleColor.Yellow;
            bool op2 = int.TryParse(Console.ReadLine(), out operand2);
            Console.ResetColor();
            Console.Write("\n\nPlease, add the desired operation (+, -, *, /,): ");
            Console.ForegroundColor = ConsoleColor.Yellow;
            bool op = char.TryParse(Console.ReadLine(), out operation);
            Console.ResetColor();

            // E -> s'assegura de que els passi bé
            while(!(op1 && op2 && op)) 
            {
                // E -> error, reask
                Console.ForegroundColor = ConsoleColor.Red;
                Console.Write("\n\nERROR. That's not a valid option. Try again");
                Console.ResetColor();

                Console.Write("\n\nPlease, add the first operand: ");
                Console.ForegroundColor = ConsoleColor.Yellow;
                op1 = int.TryParse(Console.ReadLine(), out operand1);
                Console.ResetColor();
                Console.Write("\n\nPlease, add the second operand: ");
                Console.ForegroundColor = ConsoleColor.Yellow;
                op2 = int.TryParse(Console.ReadLine(), out operand2);
                Console.ResetColor();
                Console.Write("\n\nPlease, add the desired operation (+, -, *, /,): ");
                Console.ForegroundColor = ConsoleColor.Yellow;
                op = char.TryParse(Console.ReadLine(), out operation);
                Console.ResetColor();
            }

            // E -> i retorna true (no hi ha cap situació que retorni false ja que és impossible que l'usuari introdueixi malament les operacions (es parsejen totes abans))
            return true;
        }

        public static bool VerifyPosition(int operation, Calculator cal)
        {
            // E -> hem de comprobar que la posició que ens donguin no estigui buida. Per a fer-ho, hem de mirar si la posició donada és < _numRecordedOperations
            if (operation - 1 < cal._getRecordedOperations)
            {
                return false;
            }
            else
            {
                return true;
            }
        }

        static void PressAnyKeyToContinue()
        {
            // E -> aquest procediment es fica al final del switch per a que reseteji el menú
            Console.Write("\n\nPres any key to continue");
            Console.ReadKey(true);
        }

    }
}
