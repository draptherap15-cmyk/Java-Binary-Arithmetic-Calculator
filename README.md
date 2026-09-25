import java.math.BigInteger;
import java.util.Scanner;

public class ArithmeticCalculator {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String again;

        do {
            System.out.println("\n=== Multi-Number Arithmetic Calculator ===");

            int numberCount = readNumberCount(scanner);
            int firstBase = chooseBase(scanner, "number 1");
            BigInteger result = readNumber(scanner, firstBase);

            try {
                for (int i = 2; i <= numberCount; i++) {
                    char operator = readOperator(scanner, i);
                    int base = chooseBase(scanner, "number " + i);
                    BigInteger nextNumber = readNumber(scanner, base);
                    result = calculate(result, nextNumber, operator);
                }

                int resultBase = chooseBase(scanner, "result");
                String resultText = result.toString(resultBase).toUpperCase();
                System.out.println("\nResult: " + formatResult(resultText, resultBase));

            } catch (ArithmeticException e) {
                System.out.println("Error: Cannot divide by zero.");
            }

            System.out.print("\nPerform another calculation? (Y/N): ");
            again = scanner.nextLine().trim();

        } while (again.equalsIgnoreCase("Y"));

        System.out.println("\nCalculator closed.");
        scanner.close();
    }

    private static int readNumberCount(Scanner scanner) {
        while (true) {
            System.out.print("How many numbers would you like to calculate? (2 or more): ");
            String input = scanner.nextLine().trim();

            try {
                int count = Integer.parseInt(input);
                if (count >= 2) {
                    return count;
                }
            } catch (NumberFormatException ignored) {
                // Display the shared validation message below.
            }

            System.out.println("Enter a whole number greater than or equal to 2.");
        }
    }

    private static int chooseBase(Scanner scanner, String numberName) {
        while (true) {
            System.out.println("\nChoose the base for " + numberName + ":");
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
                    System.out.println("Invalid choice. Select 2, 8, 10, or 16. Try again.");
            }
        }
    }

    private static BigInteger readNumber(Scanner scanner, int base) {
        while (true) {
            System.out.print("Enter the number: ");
            String input = scanner.nextLine()
                    .trim()
                    .replaceAll("\\s+", "");

            try {
                return new BigInteger(input, base);
            } catch (NumberFormatException e) {
                System.out.println("Invalid number for base " + base + ".");
            }
        }
    }

    private static char readOperator(Scanner scanner, int nextNumber) {
        while (true) {
            System.out.print("\nEnter an operator before number " + nextNumber + " (+, -, *, /): ");
            String input = scanner.nextLine().trim();

            if (input.length() == 1 && "+-*/".indexOf(input.charAt(0)) >= 0) {
                return input.charAt(0);
            }

            System.out.println("Invalid operator.");
        }
    }

    // Operations are evaluated from left to right, like the original two-number calculator.
    private static BigInteger calculate(BigInteger first, BigInteger second, char operator) {
        switch (operator) {
            case '+':
                return first.add(second);
            case '-':
                return first.subtract(second);
            case '*':
                return first.multiply(second);
            case '/':
                if (second.equals(BigInteger.ZERO)) {
                    throw new ArithmeticException("Division by zero");
                }
                return first.divide(second);
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