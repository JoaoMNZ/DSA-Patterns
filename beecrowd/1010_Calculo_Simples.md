import java.io.IOException;
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.util.StringTokenizer;

public class Main {
 
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        double result = 0;
        for(int i = 0 ; i < 2 ; i++){
            StringTokenizer st = new StringTokenizer(bf.readLine());
            st.nextToken();
            int quantidade = Integer.parseInt(st.nextToken());
            double valor = Double.parseDouble(st.nextToken());
            
            result = result + quantidade * valor;
        }
        System.out.printf("VALOR A PAGAR: R$ %.2f%n", result);
    }
 
}
