# <a href="https://judge.beecrowd.com/pt/problems/view/1212" target="_blank">1212 - Aritmética Primária</a>

### Problem Statement
As crianças são ensinadas a adicionar vários dígitos da direita para a esquerda, um dígito de cada vez. Muitos acham a operação "vai 1" (em inglês chamada de "carry", na qual o valor 1 é carregado de uma posição para ser adicionado ao dígito seguinte) um desafio significativo. Seu trabalho é para contar o número de operações de carry para cada um dos problemas de adição apresentados para que os educadores possam avaliar a sua dificuldade.

### Implementation
```java
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        while(true){
            StringTokenizer st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            
            if(a == 0 && b == 0){
                break;
            }
            
            int carries = 0;
            int carry = 0;
            
            while(a > 0 || b > 0){
                int sum = (a % 10) + (b % 10) + carry;
                
                carry = (sum >= 10) ? 1 : 0;
                if(carry == 1){
                  carries++;
                }
                
                a /= 10;
                b /= 10;
            }
            
            System.out.println(carries == 0 ? "No carry operation." : carries + " carry operation" + (carries > 1 ? "s." : "."));
        }
    }
}
```
