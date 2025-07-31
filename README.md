Part-1
# SafeUnsafe
import java.util.*;

public class safeUnsafe {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> reports = new ArrayList<>();

        System.out.println("Enter each report line (press Enter after each). Type 'END' to finish:");

        while (true) {
            String line = scanner.nextLine().trim();
            if (line.equalsIgnoreCase("END")) break;
            if (!line.isEmpty()) reports.add(line);
        }

        int safeReports = 0;
        for (String report : reports) {
            if (isSafe(report)) {
                safeReports++;
            }
        }

        System.out.println("Total Safe Reports: " + safeReports);
    }

    public static boolean isSafe(String report) {
        String[] parts = report.trim().split("\\s+");
        int[] levels = Arrays.stream(parts).mapToInt(Integer::parseInt).toArray();

        if (levels.length < 2) return false;

        int firstDiff = levels[1] - levels[0];
        if (firstDiff == 0 || Math.abs(firstDiff) > 3) return false;

        boolean isIncreasing = firstDiff > 0;

        for (int i = 1; i < levels.length; i++) {
            int diff = levels[i] - levels[i - 1];

            if (diff == 0 || Math.abs(diff) > 3) return false;
            if (isIncreasing && diff < 0) return false;
            if (!isIncreasing && diff > 0) return false;
        }

        return true;
    }
}
//output 242 safe 

part-2 #unsafeSafe
import java.util.*;

public class unsafeSafe {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> inputs = new ArrayList<>();

        System.out.println("Enter level reports (empty line to finish):");

        while (true) {
            String line = scanner.nextLine().trim();
            if (line.isEmpty()) break;
            inputs.add(line);
        }

        int safeCount = 0;
        int reportIndex = 1;

        for (String line : inputs) {
            int[] levels = parseLevels(line);

            if (isSafe(levels)) {
                System.out.println(line + ": Safe without removing any level.");
                safeCount++;
            } else if (canBeSafeWithOneRemoval(levels)) {
                int removalIndex = getSafeRemovalIndex(levels);
                System.out.println(line + ": Safe by removing the " + ordinal(removalIndex + 1) + " level, " + levels[removalIndex] + ".");
                safeCount++;
            } else {
                System.out.println(line + ": Unsafe regardless of which level is removed.");
            }

            reportIndex++;
        }

        System.out.println("Number of safe reports: " + safeCount);
        scanner.close();
    }

    private static int[] parseLevels(String line) {
        String[] parts = line.trim().split("\\s+");
        int[] nums = new int[parts.length];
        for (int i = 0; i < parts.length; i++) {
            nums[i] = Integer.parseInt(parts[i]);
        }
        return nums;
    }

    private static boolean isSafe(int[] arr) {
        if (arr.length < 2) return false;

        boolean increasing = arr[1] > arr[0];
        for (int i = 1; i < arr.length; i++) {
            int diff = arr[i] - arr[i - 1];
            if (Math.abs(diff) < 1 || Math.abs(diff) > 3) return false;
            if (increasing && diff < 0) return false;
            if (!increasing && diff > 0) return false;
        }
        return true;
    }

    private static boolean canBeSafeWithOneRemoval(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            int[] modified = removeIndex(arr, i);
            if (isSafe(modified)) return true;
        }
        return false;
    }

    private static int getSafeRemovalIndex(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            int[] modified = removeIndex(arr, i);
            if (isSafe(modified)) return i;
        }
        return -1; // Should not happen if called correctly
    }

    private static int[] removeIndex(int[] arr, int index) {
        int[] result = new int[arr.length - 1];
        for (int i = 0, j = 0; i < arr.length; i++) {
            if (i != index) {
                result[j++] = arr[i];
            }
        }
        return result;
    }

    private static String ordinal(int number) {
        if (number % 100 >= 11 && number % 100 <= 13) return number + "th";
        return switch (number % 10) {
            case 1 -> number + "st";
            case 2 -> number + "nd";
            case 3 -> number + "rd";
            default -> number + "th";
        };
    }
}
output-Number of safe reports: 311
