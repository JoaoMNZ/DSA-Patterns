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
