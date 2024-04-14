# Engenharia-De-Software---Principios-SOLID

 ## Instruções da atividade:

  1. Escolham 4 dos 7 princípios abaixo
  
  2. Vocês deve commitar e documentar o código no seu repositório no GitHub. Utilize o Readme do projeto para linkar os códigos e documentar a explicação do exemplo.
  
  - O que é? Para que serve?
  
  - Exemplo que ilustre a sua importância (escolha a linguagem que desejar). Explique o código detalhadamente e onde o princípio esta sendo usado e qual pooblema ele tem resolvido.
  
  3. Você deve enviar o link do seu repositório nesta Tarefa
  
  Vocês poderão escolher entre os 7 seguintes princípios (SOLID +2):
  
  S — Single Responsiblity Principle (Princípio da responsabilidade única)
  O — Open-Closed Principle (Princípio Aberto-Fechado)
  L — Liskov Substitution Principle (Princípio da substituição de Liskov)
  I — Interface Segregation Principle (Princípio da Segregação da Interface)
  D — Dependency Inversion Principle (Princípio da inversão da dependência)
  Prefira Composição a Herança
  Demeter 


  Os princípios escolhidos foram:
- Single responsibility Principle;
- Princípio de Demeter;
- Inversão da dependência;
- Prefira composição à Herança.

  ## Single Responsibility Principle:
  ### O que é?
  O Princípio da Responsabilidade Única define que uma classe deve ter apenas uma responsabilidade, e todos os seus métodos devem se referir a essa responsabilidade.

  ### Exemplo:
```cpp
  class Turma() {
  private:
  //valores de Turma
  public:
  //métodos de Turma
    int cancelaMatricula(string ra){
        for(int i = 0; i < qtde; i++) {
            if(this->alunos[i]->getRa() == ra) {
                delete this->alunos[i];
                for(int j = i + 1; j < qtde; j++) {
                    this->alunos[i] = alunos[j];
                }
                qtde--;
            return true;
            }
        }
        cout << "colocar aqui uma mensagem de erro" << endl;
        return -1;
    }
```
  O método cancelaMatricula estar dentro da classe Turma não faz sentido. Isso fere o princípio da responsabilidade única, pois todos os métodos de Turma deveriam ser relacionados à turma, portanto, uma nova classe chamada Matricula poderia ser criada:

### Utilizando o princípio:

#### turma.h   :
#include <iostream>
#include <string>
#include <stdlib.h>
using namespace std;

class Turma {
private:
    string codTurma;
    string semestre;

public:
    Turma(string semestre, int tamVetor){
        this->semestre = semestre;
        this->codTurma = "";
    }

    ~Turma() {
    }

    void mudarCodTurma(string codTurma) {
        this->codTurma = codTurma;
    }

};

#### aluno.h   :

#include <stdlib.h>
#include <iostream>
#include <string>

using namespace std;

class Aluno {
  private:
   string ra;
   string nome;

  public:
   Aluno() {
      this->ra = "0";
      this->nome = "undefined";
   }

   Aluno(string _ra, string _nome) {
      this->ra = _ra;
      this->nome = _nome;
   }

   string getRa(){
     return this->ra;
   }

   void setRa(string ra){
     this->ra = ra;
     }

   void imprimir() {
      cout << "(" << this->ra << ", " << this->nome << ")" << endl;
   }
};

#### matricula.h   :

#include <stdlib.h>
#include <iostream>
#include <string>
#include "aluno.h";
#include "turma.h";

using namespace std;

class Matricula{
private:
    int tamVetor;
    int qtde;
    Aluno** alunos;
public:
    Matricula() {
        this->tamVetor = tamVetor;
        this->qtde = 0;
        this->alunos = new Aluno*[this->tamVetor];
    }
    bool matricula(Aluno* a) {
        if(a == nullptr) {
            cout << "esse aluno não existe" << endl;
            return false;
        }
        if(qtde >= tamVetor) {
            cout << "limite de alunos atingido" << endl;
            return false;
        }
        this->alunos[this->qtde] = a;
        qtde++;
        return true;
    }

    int cancelaMatricula(string ra){
        for(int i = 0; i < qtde; i++) {
            if(this->alunos[i]->getRa() == ra) {
                delete this->alunos[i];
                for(int j = i + 1; j < qtde; j++) {
                    this->alunos[i] = alunos[j];
                qtde--;
            return true;
            }
        }
        cout << "colocar aqui uma mensagem de erro" << endl;
        return -1;
    }

    void imprimeMatriculados() {
        cout << "lista de alunos: " << endl;
        for(int i = 0; i < qtde; i++) {
            alunos[i]->imprimir();
        }
    }

    ~Matricula() {
        for (int i = 0; i < qtde; i++) {
            delete alunos[i];
        }
        delete[] alunos;
    }

};

## Princípio de Demeter e Inversão de responsabilidade:
### O que são?

  Para ilustrar, escolhe-se o seguinte código:
    void sendMail(ContaBancaria conta, String msg) {
      Cliente cliente = conta.getCliente();
      String endereco = cliente.getMailAddress();
      "Envia mail"
    }  

  Esse código fere o princípio de Demeter, do encapsulamento, pois ele utiliza de Cliente, uma classe cujo objeto não foi passado como parâmetro; uma das métricas necessárias para o bom encapsulamento de um código.
  Ele também fere o princípio de inversão de responsabilidade: módulos de alto nível não podem depender de outros de baixo nível, e que ambos devem depender somente de abstrações. Ele fere esse princípio pois o método sendEmail depende diretamente da implementação da classe Cliente pra conseguir o endereço de e-mail, ao invés de depender de uma abstração.

### Utilizando os princípios:

#### cliente.h   :
#include <iostream>
#include <string>
#include <stdlib.h>
using namespace std;


class Cliente {
    //assumindo que a classe de cliente contém esses objetos:
private:
    string nome;
    string cpf;
    string endereco;
    Cliente** cliente;
    int tamVetor;
    int posicao; //total ou a posição do momento

    Cliente(string nome, string cpf, string endereco, int tamVetor) {
        this->nome = nome;
        this->cpf = cpf;
        this->endereco = endereco;
        this->cliente = new Cliente*[tamVetor];
        int posicao = 0;
    }
    //informações de cliente aqui;

    Cliente getCliente() {
        for(int i = 0; i < posicao; i++) {
            if(this->cliente[i].getPosicao() == posicao) {
                return (*cliente[i]);
            }
        }
    }

    ~Cliente() {
        //depois de deletar o objeto na heap com todas as informações de cliente dentro de um for:
        delete [] cliente;
    }

    //outros métodos aqui;
};

#### conta.h   :

#include <iostream>
#include <string>
#include <stdlib.h>
#include "cliente.h"
#include "email.h"
using namespace std;

class ContaBancaria {
private:
    ContaBancaria* contaBancaria;
    string mailAddress;
    ContaBancaria* cliente;

public:
    ContaBancaria(string mailAddress, ContaBancaria* contaBancaria) {
        this->contaBancaria = contaBancaria;
        this->mailAddress = contaBancaria->getMailAddress();
    }

    ContaBancaria* getContaBancaria() {
        return contaBancaria;
    }

    ContaBancaria* setContaBancaria() {
        this->contaBancaria = contaBancaria;
    }

    void sendEmail(ContaBancaria cliente, string mensagem) {
        this->cliente = this->getCliente();
        //"mandar e-mail";
    }

};

#### email.h   :

#include <iostream>
#include <string>
#include "cliente.h"
#include "conta.h"
using namespace std;

class Email {
private:
    string msg;
    string mailAddress;
public:
    Email() {
        this->mailAddress = mailAddress;
        this->msg = msg;
    }

    string getMailAddress() {
        return this->mailAddress;
    }

    void setMailAddress(string mailAddress) {
        this->mailAddress = mailAddress;
    }

    void sendEmail(string conta, string msg) {
        cout << "Mandando e-mail para: " << conta.getCliente() << "Com a seguinte mensagem:" << msg << endl;
    }
};

## Preferir Composição à Herança:
### O que é?
  Normalmente, dadas duas soluções de projeto onde uma baseia-se em herança e a outra em composição, a solução por meio de composição geralmente é melhor.
  Utilizaremos o seguinte código:

#### heranca.h   :

class Comida{
private:
    string fruta;
    string legume;
    string grao;
    string proteina;

public:
    Comida() {
        this->fruta = fruta;
        this->legume = legume;
        this->grao = grao;
        this->proteina = proteina;
    }

    //métodos setteres e getteres do restante dos atributos aqui;
    string setFruta() {
        this->fruta = fruta;
    }

};

class Heranca : public Comida {
public:
Heranca() {
    setFruta();
}
};

#### composicao.h   :

class Composicao {
private:
    Comida fruta;
public:
    Composicao(){
        this->fruta = fruta;
    }

    void setFrutaComposicao(string nomeFruta) {
        this->fruta = fruta;
    }  

};

Dessa forma, nossa classe será composta somente dos métodos e atributos desejados, ao invés de herdar tudo que a classe Comida possui; por exemplo, nós não utilizamos de 'legume', 'grao' e 'proteina' na nossa classe Composicao por não julgarmos necessário, portanto ela não possui esses atributos, enquanto que a classe Heranca herdou todos esses atributos, mesmo eles sendo inúteis para o que desejamos fazer com a classe no momento.










