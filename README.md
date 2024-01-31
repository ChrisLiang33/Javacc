# javacc

options {
    STATIC = false;
}

PARSER_BEGIN(MiniJavaLex)

public class MiniJavaLex {
    public static void main(String args[]) throws ParseException {
        MiniJavaLex parser = new MiniJavaLex(System.in);
        while (true) {
            Token token = parser.getNextToken();
            if (token.kind == MiniJavaLexConstants.EOF) {
                break;
            }
            System.out.println(token.image);
        }
    }
}

PARSER_END(MiniJavaLex)

TOKEN : {
    < INTEGER_LITERAL : ("1"-"9")("0"-"9")_ | "0" >
|   < #LETTER : ["a"-"z", "A"-"Z"] >
|   < #DIGIT : ["0"-"9"] >
|   < IDENTIFIER : <LETTER> (<LETTER> | <DIGIT>)_ >
|   < #SINGLE_LINE_COMMENT : "//" (~["\n","\r"])_ ("\n"|"\r"|"\r\n")? >
|   < #MULTI_LINE_COMMENT : "/_" ~"_/" "_/" >
}

/_ Keywords _/
TOKEN : {
    < CLASS : "class" >
|   < PUBLIC : "public" >
|   < STATIC : "static" >
|   < VOID : "void" >
|   < MAIN : "main" >
|   < STRING : "String" >
|   < EXTENDS : "extends" >
|   < RETURN : "return" >
|   < INT : "int" >
|   < BOOLEAN : "boolean" >
|   < IF : "if" >
|   < ELSE : "else" >
|   < WHILE : "while" >
|   < LENGTH : "length" >
|   < TRUE : "true" >
|   < FALSE : "false" >
|   < THIS : "this" >
|   < NEW : "new" >
|   < SYSTEM_OUT_PRINTLN : "System.out.println" >
}

/_ Operators and separators _/
TOKEN : {
    < ASSIGN : "=" >
|   < COMMA : "," >
|   < DOT : "." >
|   < SEMICOLON : ";" >
|   < LEFT_PARENTHESIS : "(" >
|   < RIGHT_PARENTHESIS : ")" >
|   < LEFT_BRACE : "{" >
|   < RIGHT_BRACE : "}" >
|   < LEFT_BRACKET : "[" >
|   < RIGHT_BRACKET : "]" >
|   < AND : "&&" >
|   < LESS : "<" >
|   < ADD : "+" >
|   < SUBTRACT : "-" >
|   < MULTIPLY : "\*" >
}

Test case with all possible tokens:
class Main {
    public static void main(String[] args) {
        System.out.println(new Test().start());
    }
}

class Test {
    int[] numbers;
    boolean flag;

public int start() {
        int length;
        numbers = new int[10];
        length = numbers.length;
        numbers[0] = 1;
        flag = true;
        if (flag && length < 10) {
            System.out.println(0);
        } else {
            System.out.println(1);
        }
        return 99;
    }
}

Test case with lexical error
class Main {
    public static void main(String[] args) {
        System.out.println(new Test().start());
    }
}

class Test {
    int[] numbers;
    boolean flag;

public int start() {
        numbers = new int[10];
        flag = true;
        // Introducing lexical error with an atypical symbol: `@`
        int @error = 0;
        return numbers[0];
    }
}
