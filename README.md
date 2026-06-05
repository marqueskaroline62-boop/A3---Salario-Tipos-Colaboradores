import java.util.ArrayList;
import java.util.Scanner;

//classe abstrata
abstract class Colaborador {
    private String nome;
    private int matricula;

    // Constante salário base em 2mil
    protected static final double SALARIO_BASE = 2000.00;

    public Colaborador(String nome, int matricula) {
        this.nome = nome;
        this.matricula = matricula;
    }

    public String getNome() {
        return nome;
    }

    public int getMatricula() {
        return matricula;
    }

    // Método abstrato que as classes filhas são obrigadas a implementar
    public abstract double calcularSalarioFinal();
}

// classes dos 3 tipos de trabalhadores

// comum: recebe 2mil de base
class ColaboradorPadrao extends Colaborador {
    public ColaboradorPadrao(String nome, int matricula) {
        super(nome, matricula);
    }

    @Override
    public double calcularSalarioFinal() {
        return SALARIO_BASE;
    }
}

// o de comissão: recebe salario base + 5% de comissão
class ColaboradorComissionado extends Colaborador {
    private double totalVendas;
    private double taxaComissao;

    public ColaboradorComissionado(String nome, int matricula, double totalVendas, double taxaComissao) {
        super(nome, matricula);
        this.totalVendas = totalVendas;
        this.taxaComissao = taxaComissao;
    }

    @Override
    public double calcularSalarioFinal() {
        return SALARIO_BASE + (this.totalVendas * this.taxaComissao);
    }
}

// o de produção salário base + bônus por peças
class ColaboradorProducao extends Colaborador {
    private int qtdPecas;
    private double valorPorPeca;

    public ColaboradorProducao(String nome, int matricula, int qtdPecas, double valorPorPeca) {
        super(nome, matricula);
        this.qtdPecas = qtdPecas;
        this.valorPorPeca = valorPorPeca;
    }

    @Override
    public double calcularSalarioFinal() {
        return SALARIO_BASE + (this.qtdPecas * this.valorPorPeca);
    }
}

// menu
public class SalarioTiposColaboradores {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // lista dos funcionários
        ArrayList<Colaborador> listaColaboradores = new ArrayList<>();

        int opcao = 0;

        // ordem do menu
        while (opcao != 5) {
            System.out.println("1- Cadastro Colaborador Padrão");
            System.out.println("2- Cadastro Colaborador Comissionado");
            System.out.println("3- Cadastro Colaborador por Produção");
            System.out.println("4- Exibir Folha de Pagamento");
            System.out.println("0- Sair do Sistema");
            System.out.print("Escolha uma opção: ");

            opcao = sc.nextInt();
            sc.nextLine();

            switch (opcao) {
                case 1:
                    System.out.print("Nome: ");
                    String nomePadrao = sc.nextLine();

                    System.out.print("Matrícula: ");
                    int matPadrao = sc.nextInt();

                    // para números negativos
                    if (matPadrao < 0) {
                        System.out.println("Matrícula não pode ser negativo.");
                        break;
                    }

                    listaColaboradores.add(new ColaboradorPadrao(nomePadrao, matPadrao));
                    System.out.println("cadastrado!");
                    break;

                case 2:
                    System.out.print("Nome: ");
                    String nomeComissao = sc.nextLine();

                    System.out.print("Matrícula: ");
                    int matComissao = sc.nextInt();

                    System.out.print("Valor total de vendas realizadas: R$ ");
                    double vendas = sc.nextDouble();

                    System.out.print("Taxa de comissão (ex: pra 5% escreva 0,005 ao invés de 0.005): ");
                    double taxa = sc.nextDouble();

                    // Validação contra dados negativos usando condicionais (if)
                    if (matComissao < 0 || vendas < 0 || taxa < 0) {
                        System.out.println("Não é permitido valores negativos.");
                        break;
                    }

                    listaColaboradores.add(new ColaboradorComissionado(nomeComissao, matComissao, vendas, taxa));
                    System.out.println("cadastrado!");
                    break;

                case 3:
                    System.out.print("Nome: ");
                    String nomeProd = sc.nextLine();

                    System.out.print("Matrícula: ");
                    int matProd = sc.nextInt();

                    System.out.print("Quantidade de peças produzidas: ");
                    int pecas = sc.nextInt();

                    System.out.print("Valor extra pago por peça: R$ ");
                    double valorPeca = sc.nextDouble();

                    // Validação contra dados negativos usando condicionais (if)
                    if (matProd < 0 || pecas < 0 || valorPeca < 0) {
                        System.out.println("Não é permitido valores negativos.");
                        break;
                    }

                    listaColaboradores.add(new ColaboradorProducao(nomeProd, matProd, pecas, valorPeca));
                    System.out.println("cadastrado!");
                    break;

                case 4:
                    // exibição da folha de pagamento
                    System.out.println("Folha de pagamento:");

                    if (listaColaboradores.isEmpty()) {
                        System.out.println("Nenhum colaborador cadastrado no sistema.");
                    } else {
                        double totalFolha = 0;

                        // percorrer a lista
                        for (Colaborador c : listaColaboradores) {
                            double salarioFinal = c.calcularSalarioFinal();
                            totalFolha += salarioFinal;

                            System.out.printf("Matrícula: %-5d | Nome: %-15s | Salário Final: R$ %,.2f%n",
                                    c.getMatricula(), c.getNome(), salarioFinal);
                        }

                    }
                    break;

                case 0:
                    System.out.println("\nEncerrando...");
                    break;

                default:
                    System.out.println("Opção inválida! Digite um número de 1 a 4 ou 0 para sair.");
                    break;
            }
        }

        sc.close();
    }
}
