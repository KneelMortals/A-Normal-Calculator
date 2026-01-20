# A-Normal-Calculator
i've created a normal expression evaluator.....
import java.util.Stack;
import java.util.Scanner;
public class ExpressionEvaluator{
    public static double evaluate(String expr){
        Stack<Double>numbers=new Stack<>();
        Stack<Character>operators=new Stack<>();
        int i=0;
        while(i<expr.length()){
            char ch=expr.charAt(i);
            if(ch==' '){
                i++;
                continue;
            }
            if(Character.isDigit(ch)||ch=='.'){
                StringBuilder num=new StringBuilder();
                while(i<expr.length()&&(Character.isDigit(expr.charAt(i))||expr.charAt(i)=='.')){
                    num.append(expr.charAt(i));
                    i++;
                }
                numbers.push(Double.parseDouble(num.toString()));
                continue;
            }
            if(Operator(ch)){
                while(!operators.isEmpty()&&Priority(operators.peek())>=Priority(ch)){
                    Operation(numbers,operators);
                }
                operators.push(ch);
            }
            i++;
        }
        while(!operators.isEmpty()){
            Operation(numbers,operators);
        }return numbers.pop();
    }
    private static boolean Operator(char ch){
        return ch=='+'||ch=='-'||ch=='*'||ch=='/';
    }
    private static int Priority(char op){
        if(op=='+'||op=='-')return 1;
        if(op=='*'||op=='/')return 2;
        return 0;
    }
    private static void Operation(Stack<Double>numbers,Stack<Character>operators){
        double b=numbers.pop();
        double a=numbers.pop();
        char op=operators.pop();
        double result;
        switch (op){
            case'+':result=a+b;break;
            case'-':result=a-b;break;
            case'*':result=a*b;break;
            case'/':
                if(b==0)throw new ArithmeticException("Division by zero");
                result=a/b;
                break;
            default:
                throw new IllegalStateException("Unexpected operator: "+op);
        }
        numbers.push(result);
    }
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        System.out.print("Enter The Expression: ");
        String expr=sc.nextLine();
        sc.close();
        System.out.printf("Result: %.2f",evaluate(expr));
    }
}
