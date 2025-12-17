import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
  
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        double x1 = Double.parseDouble(st.nextToken());
        double y1 = Double.parseDouble(st.nextToken());

        st = new StringTokenizer(br.readLine());
        double x2 = Double.parseDouble(st.nextToken());
        double y2 = Double.parseDouble(st.nextToken());

        double dx = x1 - x2;
        double dy = y1 - y2;

        System.out.printf("%.4f%n", Math.sqrt(dx * dx + dy * dy));
    }
    
}
