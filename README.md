# exerc-cios-2

1.1. Coloque aqui o código da sua classe Mensagem pedido na Questão 1

public abstract class Mensagem {
	
	private String texto;
	private String emailRemetente;
	private boolean anonima;
	
	public Mensagem(String texto, String emailRemetente, boolean anonima) {
		this.texto = texto;
		this.emailRemetente = emailRemetente;
		this.anonima = anonima;
		
	}

	public String getTexto() {
		return texto;
		
	}
	
	public void setTexto(String texto) {
		this.texto = texto;
		
	}
	
	public abstract String getTextoCompletoAExibir();
	
	public String getEmailRemetente() {
		return emailRemetente;
		
	}
	
	public void setEmailRemetente(String emailRemetente) {
		this.emailRemetente = emailRemetente;
		
	}
	
	public boolean ehAnonima() {
		return anonima;
		
	}

}


1.2 Coloque aqui o código da sua classe MensagemParaAlguem

public class MensagemParaAlguem extends Mensagem{
	
	private String emailDestinatario;

	public MensagemParaAlguem(String texto, String emailRemetente, String emailDestinatario, boolean anonima) {
		super(texto, emailRemetente, anonima);
		this.emailDestinatario = emailDestinatario;
	}
	
	public String getEmailDestinatario() {
		return emailDestinatario;
		
	}
	
	public void setEmailDestinatario(String emailDestinatario) {
		this.emailDestinatario = emailDestinatario;
		
	}
	
	public String getTextoCompletoAExibir() {
		if(ehAnonima() == true) {
			return "Mensagem de remetente anônimo para: "+getEmailDestinatario()+". Texto: "+getTexto();
		}else {
			return "Mensagem de: "+getEmailRemetente()+", para: "+getEmailDestinatario()+". Texto: "+getTexto();
		}
		
	}
	
}


2. Coloque aqui o código da sua classe SistemaAmigo pedido na questão 2

import java.util.ArrayList;
import java.util.List;

public class SistemaAmigo {
	
	private List<Mensagem> mensagens;
	private List<Amigo> amigos;
	
	public SistemaAmigo() {
		this.mensagens = new ArrayList<>();
		this.amigos = new ArrayList<>();
		
	}
	
	public void cadastraAmigo(String nomeAmigo, String emailAmigo) throws AmigoJaExisteException{
		
		Amigo novoAmigo = new Amigo(nomeAmigo, emailAmigo);
		if(!this.amigos.contains(novoAmigo)) {
			this.amigos.add(novoAmigo);
		}else {
			throw new AmigoJaExisteException("Já existe essa pessoa no sistema");
		}
	}
	
	public Amigo pesquisaAmigo(String emailAmigo) throws AmigoInexistenteException{
		for(Amigo a: this.amigos) {
			if(a.getEmail().equals(emailAmigo)) {
				return a;
			}
		}
		throw new AmigoInexistenteException("Não existe pessoa cadastrada com esse email: '"+emailAmigo+"'!");
	}
	
	public void enviarMensagemParaTodos(String texto, String emailRemetente, boolean ehAnonimo) {
		Mensagem mensagensEnviadas = new MensagemParaTodos(texto, emailRemetente, ehAnonimo);
		this.mensagens.add(mensagensEnviadas);
	}
	
	public void enviarMensagemParaAlguem(String texto, String emailRemetente, String emailDestinatario, boolean ehAnonimo) {
		Mensagem mensagensEnviadasAlguem = new MensagemParaAlguem(texto, emailRemetente, emailDestinatario, ehAnonimo);
		this.mensagens.add(mensagensEnviadasAlguem);
	}
	
	public List<Mensagem> pesquisaMensagensAnonimas(){
		ArrayList<Mensagem> mensagensAnonimas = new ArrayList<Mensagem>();
		for(Mensagem m: this.mensagens) {
			if(m.ehAnonima()) {
				mensagensAnonimas.add(m);
			}
		}
		return mensagensAnonimas;
	}
	
	
	
	public void configuraAmigoSecretoDe(String emailDaPessoa, String emailAmigoSorteado) throws AmigoInexistenteException{
		boolean emailExiste = false;
		for(Amigo a: this.amigos) {
			if(a.getEmail().equals(emailDaPessoa)) {
				a.setEmailAmigoSorteado(emailAmigoSorteado);
				emailExiste = true;
			}
		}
		if(emailExiste == false) {
			throw new AmigoInexistenteException("Não existe pessoa cadastrada com esse email: '"+emailDaPessoa+"'!");
		}
	}
	
	public List<Mensagem> pesquisaTodasAsMensagens(){
		ArrayList<Mensagem> mensagensEnviadas = new ArrayList<Mensagem>();
		for(Mensagem m: this.mensagens) {
			mensagensEnviadas.add(m);
		}
		return mensagensEnviadas;
	}
	
	
	public String pesquisaAmigoSecretoDe(String emailDaPessoa) throws AmigoInexistenteException, AmigoNaoSorteadoException{
		boolean emailExiste = false;
		boolean amigoSecretoExiste = false;
		String emailAmigoSecreto = "";
		for(Amigo a: this.amigos) {
			if(a.getEmail().equals(emailDaPessoa)) {
				if(!a.getEmailAmigoSorteado().equals(null)) {
					emailAmigoSecreto = a.getEmailAmigoSorteado();
					amigoSecretoExiste = true;
				}
				emailExiste = true;
			}
		}
		if(emailExiste == false) {
			throw new AmigoInexistenteException("Não existe pessoa cadastrada com esse email: '"+emailDaPessoa+"'!");
		}else if(amigoSecretoExiste == false) {
			throw new AmigoNaoSorteadoException("A pessoa cadastrada com esse email não possue 'Amigo Secreto'");
		}else {
			return emailAmigoSecreto;
		}
	}
	
	
}


3. Coloque aqui o código da classe TestaSistemaAmigo  pedida na questão 3.
import java.util.List;

public class TestaSistemaAmigo {
	
	public static void main(String [] args) {
		
		SistemaAmigo sistema = new SistemaAmigo();
		
		try {
			sistema.cadastraAmigo("José", "José.silva@dcx.ufpb.br");
			sistema.cadastraAmigo("Maria", "Maria.Barbosa@dcx.ufpb.br");
			sistema.configuraAmigoSecretoDe("José.silva@dcx.ufpb.br", "Maria.Barbosa@dcx.ufpb.br");
			sistema.enviarMensagemParaAlguem("Oi Amigo", "Maria.Barbosa@dcx.ufpb.br","José.silva@dcx.ufpb.br", true);
			sistema.enviarMensagemParaTodos("Adorando a Brincadeira", "Maria.Barbosa@dcx.ufpb.br", true);
			
			List<Mensagem> anonimas = sistema.pesquisaMensagensAnonimas();
			for(Mensagem a : anonimas) {
				System.out.println(a.getTextoCompletoAExibir());
			}
			
			String jose = sistema.pesquisaAmigoSecretoDe("José.silva@dcx.ufpb.br");
			if(jose == "Maria.Barbosa@dcx.ufpb.br") {
				System.out.println("Ok");
			}
			
		}catch (AmigoJaExisteException | AmigoInexistenteException | AmigoNaoSorteadoException e) {
			System.out.println(e.getMessage());
		}
		
	}
}


4. Coloque aqui o código da classe TestaSistemaAmigoGUI   pedida na questão 4.

import javax.swing.JOptionPane;


public class TestaSistemaAmigoGUI {

	public static void main(String [] args) {
		
		SistemaAmigo sistema = new SistemaAmigo();
		
		String perguntaMensagens = JOptionPane.showInputDialog("Qual o limite de mensagens na brincadeira?");
		double quantMensagens = Double.parseDouble(perguntaMensagens);
		
		String perguntaAmigos = JOptionPane.showInputDialog("Quantos amigos participaram da brincadeira?");
		double quantAmigos = Double.parseDouble(perguntaAmigos);
		
		for(int i =0;i <= quantAmigos; ++i) {
			String nome = JOptionPane.showInputDialog("Qual o nome do participante?");
			String email = JOptionPane.showInputDialog("Qual o email do participante?");
			String emailAmigoSorteado = JOptionPane.showInputDialog("Qual o email do amigo sorteado?");
			
			try {
				sistema.cadastraAmigo(nome, email);
				sistema.configuraAmigoSecretoDe(email, emailAmigoSorteado);
				JOptionPane.showMessageDialog(null, nome+"pegou "+sistema.pesquisaAmigo(sistema.pesquisaAmigoSecretoDe(email)).getNome()+"como amigo sorteado!");
				String mensagemAnonima = JOptionPane.showInputDialog("Deseja enviar a mensagem de introdução anonimamente?");
				if(mensagemAnonima.equalsIgnoreCase("sim")) {
					String digiteMensagem = JOptionPane.showInputDialog("Digite a sua mensagem:");
					String mensagemInicial = JOptionPane.showInputDialog(digiteMensagem);
					sistema.enviarMensagemParaTodos(mensagemInicial, email, true);
				}else {
					String digiteMensagem = JOptionPane.showInputDialog("Digite a sua mensagem:");
					String mensagemInicial = JOptionPane.showInputDialog(digiteMensagem);
					sistema.enviarMensagemParaTodos(mensagemInicial, email, false);
				}
			}catch (AmigoJaExisteException | AmigoInexistenteException | AmigoNaoSorteadoException e) {
				System.out.println(e.getMessage());
			}
		}
	}
	
	
}
