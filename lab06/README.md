## Arquivos Java do Jogo

> Para acionar o jogo, execute a classe AppMundoWumpus, que está contida na pasta Bin, e passe como argumento o local do arquivo em que está contido o CSV da caverna, que nesse caso está na pasta db. Após isso, para iniciar o jogo, insira seu nome e o primeiro movimento que você deseja fazer.
>
> [Link para a pasta](https://github.com/lucaseduoli/mc322-qota/tree/main/lab06/src/mc322/lab06)

## Destaques de Arquitetura

> Nessa parte, devem ser apresentados trechos de código de destaque em que foram adotadas soluções que atendem um ou mais dos seis critérios de qualidade listados anteriormente. Por exemplo, pode ser dado destaque a uma solução encontrada pela equipe que permita fácil expansão caso novas classes de Componente sejam criadas.

### `Fácil expansão com a inserção do componente na sala`

> Escolha recortes relevantes e/ou de destaque do seu código. O recorte de código deve ser curto (entre 5 a 10 linhas). Apresente um recorte (você pode usar reticências para remover partes menos importantes). Veja como foi usado o highlight de Java para o código.

~~~java
    public int insereComponente(Componente componente){
        if (componente.getTipo() == 'P') // única particularidade do código: se for um player, a sala é visitada.
            visitado = true;
        if(componentes[0] != null){
            int retorno = componente.verificaAcao(componentes[0].getTipo(), posX, posY);
            if(retorno == 1){ // quando o verificaAcao de cada componente retorna 1, a sala deve substituir o primeiro componente por ele.
                componentes[0] = componente;
            } else if(retorno == 2){ // quando o verificaAcao de cada componente retorna 2, a sala deve adicionar o componente na última casa e ordenar o vetor, com base nas prioridades.
                posUltimaCasa++;
                componentes[posUltimaCasa] = componente;
                ordenaVetor();
            }
            return retorno;
        } else {
            componente.verificaAcao('-', posX, posY); // é chamado para permitir que o componente atualize sua posição se necessário
            posUltimaCasa++;
            componentes[posUltimaCasa] = componente;
            ordenaVetor();
            return 2;
        }
    }
~~~

> O trecho que destacamos se refere ao método insereComponente da classe Sala, em que é explorado ao máximo o polimorfismo ao permitir que os componentes determinem o comportamento esperado ao ser inserido na sala. Nesse exemplo, pergunta-se ao componente se a ação solicitada é válida, passando como parâmetro o componente de prioridade mais alta que já está na sala. Assim, se o componente retornar 1, o primeiro componente do vetor contido em Sala é substituido por ele. Se ele retornar 2, ele é adicionado no fim da lista, e, se ele retornar 0, ele não é inserido. Isso permite uma fácil expansão do código, pois, se adicionarmos algum outro componente, basta definir seu método verificaAcao e sua prioridade para que o método insereComponente insira-o na sala corretamente, de acordo com a lógica do jogo.
