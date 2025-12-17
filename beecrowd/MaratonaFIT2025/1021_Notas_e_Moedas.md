# <a href="https://judge.beecrowd.com/pt/problems/view/1021" target="_blank">1021 - Notas e Moedas</a>

### Problem Statement
Leia um valor de ponto flutuante com duas casas decimais. Este valor representa um valor monetário. A seguir, calcule o menor número de notas e moedas possíveis no qual o valor pode ser decomposto. As notas consideradas são de 100, 50, 20, 10, 5, 2. As moedas possíveis são de 1, 0.50, 0.25, 0.10, 0.05 e 0.01. A seguir mostre a relação de notas necessárias.

### Implementation
```java
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int[] valores = new int[]{10000, 5000, 2000, 1000, 500, 200, 100, 50, 25, 10, 5, 1};

        int valor = (int) (Double.parseDouble(br.readLine()) * 100);
        System.out.println("NOTAS:");
        for(int v : valores){
          int qtd = valor / v;
          valor %= v;
          
          if(v == 100){
            System.out.println("MOEDAS:");
          }
          
          System.out.printf(qtd + (v > 100 ? " nota(s) " : " moeda(s) ") + "de R$ %.2f%n", v / 100.0);
        }
    }
}
```
