import java.text.SimpleDateFormat;
import java.util.Calendar;

public class QuarterFinder {
    public static String getQuarter(String dateStr) {
        try {
            int month = new SimpleDateFormat("dd/MM/yyyy").parse(dateStr).getMonth() + 1;
            return "Q" + ((month - 1) / 3 + 1);
        } catch (Exception e) {
            return "Invalid Date";
        }
    }

    public static void main(String[] args) {
        String dateStr = "15/05/2023";
        System.out.println("Quarter: " + getQuarter(dateStr));
    }
}