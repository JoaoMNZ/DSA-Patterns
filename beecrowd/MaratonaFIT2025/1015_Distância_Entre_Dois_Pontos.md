# <a href="https://judge.beecrowd.com/pt/problems/view/1015" target="_blank">1015 - Distância Entre Dois Pontos</a>

### Problem Statement
Leia os quatro valores correspondentes aos eixos x e y de dois pontos quaisquer no plano, p1(x1,y1) e p2(x2,y2) e calcule a distância entre eles, mostrando 4 casas decimais, segundo a fórmula:

Distancia = $\sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$

### Implementation
```java
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
```
