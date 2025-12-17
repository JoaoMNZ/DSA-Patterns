# <a href="https://judge.beecrowd.com/pt/problems/view/2466" target="_blank">2466 - Sinuca</a>

### Problem Statement


Nadine e Celine inventaram um passatempo com bolas de sinuca, pretas e brancas, que são colocadas uma por vez na mesa, de acordo com uma regra fixa. Agora elas estão tentando descobrir, com um computador, a cor da bola que vai ser colocada por último! Você pode ajuda-las?

Funciona assim. No início, são colocadas N bolas formando a primeira fileira. Em seguida, um triângulo equilátero é formado, fileira a fileira, de acordo com a seguinte regra. Ao se colocar uma bola na nova fileira, ela ficará encostada em duas bolas da fileira anterior e sua cor será:

* Preta, se estiver encostada em duas bolas de mesma cor;
* Branca, se estiver encostada em duas bolas de cores diferentes.

Nesta tarefa, você deve escrever um programa que, dadas as cores das bolas da primeira fileira, descubra qual é a cor da bola que será colocada por último.


### Implementation
```java
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
```
