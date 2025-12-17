# <a href="https://judge.beecrowd.com/pt/problems/view/1010" target="_blank">1010 - Cálculo Simples</a>

### Problem Statement
Neste problema, deve-se ler o código de uma peça 1, o número de peças 1, o valor unitário de cada peça 1, o código de uma peça 2, o número de peças 2 e o valor unitário de cada peça 2. Após, calcule e mostre o valor a ser pago.

### Implementation
```java
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
```
