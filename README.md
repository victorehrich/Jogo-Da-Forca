# Jogo-Da-Forca
Jogo da Forca criado em c++

#include <iostream>
#include <string>
#include <stdio.h>
#include <time.h>

using namespace std;
//Funçao que ira selecionar aleatoriamente uma das palavras do banco e retornar uma delas, usando para isso a funçao rand.
string RetornaPalavraAleatoria(){
    string palavras[4]={"abacaxi","morango","tomate","sorvete"};
    int indice = rand()%4;
    cout <<indice;
    return palavras[indice];
}
//funçao que ira gerar uma mascara na palavra, mostrando apenas a quantidade de letras dela;
string RetornaPalavraComMascara(string palavra,int tamanhoPalavra){
    
    
    string palavraComMascara; 
    
    for(int i=0;i<palavra.size();i++){
       palavraComMascara += "_";
    }
    return palavraComMascara;
}
//void que ira exibir o status do jogo, como numero de tentativas  e uma menssagem para a proxima letra que tera de ser exibida.
void ExibeStatus(string retornoMascara, int tamanhoPalavra,int tentativasrestantes,string letrasriscadas, string mensagem){
    cout<<mensagem<<"\n";
    cout<< retornoMascara;
    cout<<" (tamanho da palavra: "<<tamanhoPalavra<<")\n";
    cout<<"\ntentativas Restantes:"<<tentativasrestantes;
    cout<<"\nLetras Ja Riscadas: ";
    int cont;
    for(cont = 0;cont<letrasriscadas.size();cont++){
        cout<<letrasriscadas[cont]<<", ";
    }
    cout<< "\nDigite uma letra\n";
}
//funçao de jogo single player
int Jogar(int numerodejogadores){
    string palavra;
    if(numerodejogadores == 1){
        palavra = RetornaPalavraAleatoria();
    }
    else{
        cout<<"Digite uma palavra:\n";
        cin>>palavra;
    }
    //variavel que recebe a palavra aleatoria.
    
    //variavel que armazena o tamanho da palavra que esta sendo usada no jogo atual.
    int tamanhoPalavra = palavra.size();

    //variavel que ira retornar  em "_" a quantidade de letras da palavra atual do jogo.
    string retornoMascara = RetornaPalavraComMascara(palavra,tamanhoPalavra);
    
    ///variaveis principais
    int tentativas = 0,maxtentativas = 6;       //numero de tentativas e erros
    char letra;                                 //letra que o usuario digitou
    int cont = 0;                               //auxiliar para o laço de repetição
    int aux=0;                                  //auxiliar para o numero de tentativas
    string letrasriscadas;                      //acumula as tentativas do jogador
    string mensagem = "Bem Vindo Ao Jogo!!";    //Feedback do jogador
    bool jadigitou = false;                     //auxiliar para saber se a letra ja foi digitada
    int opcao;                                  //opções finais
    //variavel auxiliar descartavel
    int tentativasrestantes = maxtentativas - tentativas;
    int tentativascontador = tentativas ;
    
    //funcao que ira funcionar enquanto ainda existir tentativas a serem usadas.
    //usando a funcao exibe status, lendo a letra que o usuario digitar e reduzindo o numero de tentativas restantes.
    while(palavra != retornoMascara && tentativasrestantes > 0){
        ExibeStatus(retornoMascara,tamanhoPalavra,tentativasrestantes,letrasriscadas,mensagem);
        cin>>letra;
        
        for(cont = 0;cont<tentativascontador;cont++){
            if(letrasriscadas[cont] == letra){
                mensagem = "Ja foi digitada!!";
                
                jadigitou = true;
            }
        }
        if(jadigitou == false){
            letrasriscadas += tolower(letra);
            for(cont=0;cont<tamanhoPalavra;cont++){
                if(palavra[cont] == tolower(letra)){
                    retornoMascara[cont] = palavra[cont];
                    aux++;
                }
            }
            if(aux==0){
                tentativasrestantes--;
                mensagem = "Voce errou uma letra!!";
            }
            else{
                aux = 0;
                mensagem = "Voce acertou uma letra!!";
            }
        }
        
        system("clear");
        jadigitou = false;
        tentativascontador++;
        
    }
    
    if(palavra == retornoMascara){
        cout<<"Parabens,voce venceu!";
        cout<<"\nDeseja Reiniciar?";
        cout<<"\n1-Sim\n2-Nao\n";
        cin>>opcao;
        return opcao;
        
    }
    else{
        cout<<"Que pena, voce perdeu!";
        cout<<"\nDeseja Reiniciar?";
        cout<<"\n1-Sim\n2-Nao\n";
        cin>>opcao;
        return opcao;
    }
}
void MenuInicio(){
    system("clear");
    //auxiliar para o menu
    int menuOpcao = 0;
    //menu feito da forma que o auxiliar receba um valor entre 1 e 3 para qualquer opcao do jogo, e 4 para sair dele.
    //Outros valores nao seram aceitos.
    do{
        cout<<"Bem vindo ao Jogo Da Forca\n";
        cout<<"1 - Jogo Individual\n";
        cout<<"2 - Jogo Em Dupla\n";
        cout<<"3 - Sobre\n";
        cout<<"4 - Sair\n";
        cout<<"Escolha uma opcao e aperte ENTER\n";
        cin >> menuOpcao;
        if(menuOpcao>4){
            system("clear");
            cout<<"OPCAO INVALIDA, ESCOLHA UMA OPCAO VALIDA\n";
        }
    }while(menuOpcao >4 || menuOpcao < 1);
    system("clear");
    switch(menuOpcao){
            case 1:
                //Inicia o jogo
                if(Jogar(1) == 1){
                    MenuInicio();
                }
                break;
            
            case 2:
                //Inicia o jogo em dupla
                if(Jogar(2) == 1){
                    MenuInicio();
                }
                break;
            
            case 3:
                //Pequeno resumo sobre o jogo e o desenvolvedor
                cout<<"Informacoes do jogo:\n\n";
                system("clear");
                cout<<"Jogo Desenvolvido por Victor Ehrich - 2020\n";
                cout<<"Jogo da Forca usado como atividade para desenvolver as habilidades de programação na linguagem c++\n";
                cout<<"\n1-Voltar\n2-Sair\n";
                cin>>menuOpcao;
                if(menuOpcao == 1){
                    MenuInicio();
                }
                break;
            
            default:
                //Sai do jogo
                cout<<"Ate a proxima\n\n";
                break;
        }    
}
int main()
{
    //gera numeros aleatorios
    srand((unsigned)time(NULL));
    MenuInicio();
    return 0;
}
