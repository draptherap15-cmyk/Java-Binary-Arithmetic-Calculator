import java.util.Scanner;

public class ArithmeticCalculator {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String again;

        do {
            System.out.println("\n=== Arithmetic Calculator ===");

            // First number
            int firstBase = chooseBase(scanner, "first number");
            String firstInput = readNumber(scanner, firstBase);

            // Operator
            char operator = readOperator(scanner);

            // Second number
            int secondBase = chooseBase(scanner, "second number");
            String secondInput = readNumber(scanner, secondBase);

            // Result base
            int resultBase = chooseBase(scanner, "result");

            try {
                long firstNumber = Long.parseLong(firstInput, firstBase);
                long secondNumber = Long.parseLong(secondInput, secondBase);

                long result = calculate(firstNumber, secondNumber, operator);

                String resultText = Long.toString(result, resultBase).toUpperCase();

                System.out.println("\nResult: " + formatResult(resultText, resultBase));

            } catch (NumberFormatException e) {
                System.out.println("Error: The number is invalid for its selected base. Try again.");
            } catch (ArithmeticException e) {
                System.out.println("Error: Cannot divide by zero.");
            }

            System.out.print("\nPerform another calculation? (Y/N): ");
            again = scanner.nextLine().trim();

        } while (again.equalsIgnoreCase("Y"));

        System.out.println("\nCalculator closed.");
        scanner.close();
    }

    private static int chooseBase(Scanner scanner, String numberName) {
        while (true) {
            System.out.println("\nChoose the base for the " + numberName + ":");
            System.out.println("2 - Binary");
            System.out.println("8 - Octal");
            System.out.println("10 - Decimal");
            System.out.println("16 - Hexadecimal");
            System.out.print("Choice: ");

            String choice = scanner.nextLine().trim();

            switch (choice) {
                case "2":
                    return 2;
                case "8":
                    return 8;
                case "10":
                    return 10;
                case "16":
                    return 16;
                default:
                    System.out.println("Invalid choice. Select 2, 8, 10, or 16. Try again");
            }
        }
    }

    private static String readNumber(Scanner scanner, int base) {
        while (true) {
            System.out.print("Enter the number: ");
            String input = scanner.nextLine()
                    .trim()
                    .replaceAll("\\s+", "");

            try {
                Long.parseLong(input, base);
                return input;
            } catch (NumberFormatException e) {
                System.out.println("Invalid number for base " + base + ".");
            }
        }
    }

    private static char readOperator(Scanner scanner) {
        while (true) {
            System.out.print("\nEnter an operator (+, -, *, /): ");
            String input = scanner.nextLine().trim();

            if (input.length() == 1 &&
                    (input.charAt(0) == '+' ||
                     input.charAt(0) == '-' ||
                     input.charAt(0) == '*' ||
                     input.charAt(0) == '/')) {
                return input.charAt(0);
            }

            System.out.println("Invalid operator.");
        }
    }

    private static long calculate(long first, long second, char operator) {
        switch (operator) {
            case '+':
                return first + second;

            case '-':
                return first - second;

            case '*':
                return first * second;

            case '/':
                if (second == 0) {
                    throw new ArithmeticException();
                }
                return first / second;

            default:
                throw new IllegalArgumentException("Invalid operator.");
        }
    }

    private static String formatResult(String result, int base) {
        switch (base) {
            case 2:
                return "(" + result + ")₂";

            case 8:
                return "(" + result + ")₈";

            case 16:
                return "(" + result + ")₁₆";

            case 10:
                return result;

            default:
                return result;
        }
    }
}