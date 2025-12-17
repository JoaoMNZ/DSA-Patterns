import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        
        StringTokenizer st = new StringTokenizer(br.readLine());
        int[] fileira = new int[n];
        for (int i = 0; i < n; i++) {
            fileira[i] = Integer.parseInt(st.nextToken());
        }

        for (int tamanho = n; tamanho > 1; tamanho--) {
            for (int i = 0; i < tamanho - 1; i++) {
                fileira[i] = fileira[i] * fileira[i + 1];
            }
        }

        System.out.println(fileira[0] == 1 ? "preta" : "branca");
    }
}

