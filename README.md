import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Cifrar_e_Decifrar {

    // *Cifra de César*: É uma forma de cifra por substituição, onde cada letra da mensagem original é trocada
    // por outra letra do alfabeto seguindo um deslocamento fixo.
    public static String cesarEncrypt(String text, int shift) {
        StringBuilder sb = new StringBuilder();
        for (char c : text.toCharArray()) {
            if (Character.isLetter(c)) {
                char base = Character.isLowerCase(c) ? 'a' : 'A';
                c = (char) ((c - base + shift + 26) % 26 + base);
            }
            sb.append(c);
        }
        return sb.toString();
    }

    public static String cesarDecrypt(String text, int shift) {
        return cesarEncrypt(text, -shift);
    }

    // *Cifra de Substituição*: deslocamento é o número de “casas” que cada letra da mensagem, 
    // vai andar no alfabeto durante a cifragem no Cifra de César.
    private static final Map<Character, Character> subMap = new HashMap<>();
    private static final Map<Character, Character> subReverse = new HashMap<>();

    static {
        String plain = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        String cipher = "QWERTYUIOPASDFGHJKLZXCVBNM";
        for (int i = 0; i < plain.length(); i++) {
            subMap.put(plain.charAt(i), cipher.charAt(i));
            subReverse.put(cipher.charAt(i), plain.charAt(i));
        }
    }

    public static String substitutionEncrypt(String text) {
        StringBuilder sb = new StringBuilder();
        text = text.toUpperCase();
        for (char c : text.toCharArray()) {
            sb.append(subMap.getOrDefault(c, c));
        }
        return sb.toString();
    }

    public static String substitutionDecrypt(String text) {
        StringBuilder sb = new StringBuilder();
        text = text.toUpperCase();
        for (char c : text.toCharArray()) {
            sb.append(subReverse.getOrDefault(c, c));
        }
        return sb.toString();
    }

    // *MD5*: Pega uma mensagem e gera um “resumo” único dela.
    public static String generateMD5(String text) {
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] hash = md.digest(text.getBytes());
            StringBuilder sb = new StringBuilder();
            for (byte b : hash) sb.append(String.format("%02x", b));
            return sb.toString();
        } catch (NoSuchAlgorithmException e) {
            return "Erro: MD5 indisponível";
        }
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n=== CIFRAR E DECIFRAR ===");
            System.out.println("1 - Cifra de César ");
            // ex1 = Substituí pela próxima letra do alfabeto. ex2 = Substituída pela 2 posição à frente no alfabeto. 3 = Casas você vai andar no alfabeto.
            System.out.println("2 - Substituição "); 
            // Cada letra do alfabeto é trocada por outra letra fixa.
            System.out.println("3 - Gerar MD5");
            // Devolve um código fixo de 32 caracteres hexadecimais.
            System.out.println("0 - Sair");
            System.out.print("Escolha: ");

            String line = sc.nextLine().trim();
            int op;
            try {
                op = Integer.parseInt(line);
            } catch (NumberFormatException ex) {
                System.out.println("Entrada inválida. Digite um número (0-3).");
                continue;
            }

            if (op == 0) {
                System.out.println("Encerrado, obrigado!");
                break;
            }

            switch (op) {
                case 1:
                    System.out.print("Mensagem: ");
                    String msg1 = sc.nextLine();
                    System.out.print("Deslocamento (escolha: 1, 2 ou 3): ");
                    int shift;
                    try {
                        shift = Integer.parseInt(sc.nextLine().trim());
                    } catch (NumberFormatException e) {
                        System.out.println("Deslocamento inválido.");
                        break;
                    }
                    String enc1 = cesarEncrypt(msg1, shift);
                    String dec1 = cesarDecrypt(enc1, shift);
                    System.out.println("Cifrada:   " + enc1);
                    System.out.println("Decifrada: " + dec1);
                    break;

                case 2:
                    System.out.print("Mensagem: ");
                    String msg2 = sc.nextLine();
                    String enc2 = substitutionEncrypt(msg2);
                    String dec2 = substitutionDecrypt(enc2);
                    System.out.println("Cifrada:   " + enc2);
                    System.out.println("Decifrada: " + dec2);
                    break;

                case 3:
                    System.out.print("Texto para MD5: ");
                    String msg3 = sc.nextLine();
                    System.out.println("MD5: " + generateMD5(msg3));
                    System.out.println("Decifrada: " + msg3);
                    break;

                default:
                    System.out.println("Opção inválida.");
            }
        }

        sc.close();
    }
}
