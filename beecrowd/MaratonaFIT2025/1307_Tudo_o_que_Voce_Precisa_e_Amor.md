# <a href="https://judge.beecrowd.com/pt/problems/view/1307" target="_blank">1307 - Tudo o que Você Precisa é Amor</a>

### Problem Statement
Foi inventado um novo dispositivo poderoso pela Beautifull Internacional Machines Corporation chamado de "Máquina do amor!". Dada uma string feita de dígitos binários, a máquina do amor responde se isto é feito somente de amor, ou seja, se tudo o que você irá precisar para construir aquela string for somente amor. A definição de amor para a Máquina do amor é outra string de dígitos binários, fornecida por um operador humano. Vamos supor que nós temos uma string L que representa "love" e forneçamos uma string S para a máquina do amor. Diremos então que tudo o que você precisa é amor para construir S se pudermos repetidamente subtrair L de S até que sobre apenas L. A subtração definida aqui é a mesma subtração aritmética binária na base 2. Por definição é fácil de ver que L > S (em binário), então S não é feito de amor. Se S = L então S é obviamente feito de amor.

Por exemplo, suponha S = "11011" e L = "11". Se repetidamente subtrairmos L de S, obteremos: 11011, 11000, 10101, 10010, 1111, 1100, 1001, 110, 11. Portanto, dado este L, tudo o que você necessita é amor para construir S. Devido a algumas limitações da Máquina do Amor, não será possível lidar com strings com zero à esquerda. Por exemplo "0010101", "01110101", "011111" etc. são string Inválidas. Strings que contenham apenas um dígito também são strings inválidas (isto é outra limitação).

Sua tarefa para este problema é: dadas duas strings binárias válidas, S1 e S2, veja se é possível ter uma string L válida tal que ambas, S1 e S2 possam ser feitas apenas de L (i.e. dadas duas strings válidas S1 e S2, indique se existe pelo menos uma string L válida tal que ambas S1 e S2 sejam feitas apenas de L). Por exemplo, para S1 = 11011 e S2 = 11000, nós podemos ter L = 11 tal que S1 e S2 são feitas ambas somente de L (como pode ser visto no exemplo abaixo).

### Implementation
```java
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
```
