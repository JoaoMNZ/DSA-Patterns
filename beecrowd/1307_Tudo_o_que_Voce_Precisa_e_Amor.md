import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
 
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        
        for(int i = 1 ; i <= n ; i++){
          int a = Integer.parseInt(br.readLine(), 2);
          int b = Integer.parseInt(br.readLine(), 2);
          
          while(b != 0){
            int resto = a % b;
            a = b;
            b = resto;
          }
          
          System.out.println("Pair #" + i + ": " + (a > 1 ? "All you need is love!" : "Love is not all you need!"));
        }
    }
 
}
