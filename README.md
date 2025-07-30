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
