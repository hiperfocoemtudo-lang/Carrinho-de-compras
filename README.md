# Carrinho

Uma cesta flutuante para juntar arquivos de várias pastas e entregá-los de uma vez. Feito para **Linux Mint com Cinnamon, em uma sessão X11**. A janela de configurações usa um layout claro, com a barra de título do sistema.

Inclui duas pré-definições completas no próprio programa:

- **Carrinho de compras**: imagem de colagem, controles e opções originais.
- **Steeve**: imagem, ícones, posições, tamanhos e demais opções da configuração Steeve incluída nesta versão.

As imagens estão em `assets/` e as definições de fábrica em `presets.json`. Não é necessário importar arquivos ou possuir a pasta pessoal do autor. Em uma instalação nova, Steeve é a configuração inicial. Atualizações preservam as configurações e pré-definições pessoais existentes.

## 1. Requisitos e dependências

Ambiente suportado e testado: **Linux Mint 22.x, Cinnamon, X11**, com compositor habilitado. Não é um aplicativo Windows, macOS ou Wayland. Outros desktops não são suportados pelo instalador: a integração de atalhos usa os esquemas do Cinnamon.

| Pacote ou componente | Para que serve |
| --- | --- |
| `python3` (3.10 ou superior; testado com 3.12) | Executar o aplicativo |
| `python3-gi` | Acessar GTK, GDK, GdkPixbuf, Gio e GLib |
| `gir1.2-gtk-3.0` | Bibliotecas e introspecção GTK 3.24 |
| `python3-cairo` | Desenhar as imagens e animações |
| `python3-gi-cairo` | Integrar Cairo com GTK |
| `python3-xlib` | Detectar o gesto de sacudir e integrar o arraste X11 |
| `libglib2.0-bin` | Ferramentas GLib/GSettings para diagnóstico |
| `systemd` e sessão de usuário ativa | Manter o serviço de gestos |
| `dbus-user-session` | Comunicar com a sessão, o Cinnamon e a instância do aplicativo |
| Cinnamon e seus esquemas GSettings | Registrar Ctrl + Super + C |
| Servidor X11 e compositor | Transparência, janela flutuante e arraste |
| Shell POSIX e ferramentas básicas do sistema, incluindo `readlink`, `dirname` e `id` | Executar os scripts |

**Nemo** é o gerenciador de arquivos usado para testar a entrega em pastas por XDS. Ele já acompanha o Cinnamon no Mint. Outros destinos precisam implementar os formatos/protocolos de arraste aceitos pelo aplicativo.

Não são necessários Node.js, pip, uma virtualenv, Electron, compilador ou serviços na nuvem. As bibliotecas da interface são instaladas pelo gerenciador de pacotes do sistema. Os módulos Python usados além dos pacotes acima fazem parte da biblioteca padrão.

## 2. Baixar o código

### Pelo site do GitHub

1. Na página deste repositório, clique em **Code → Download ZIP**.
2. Extraia o ZIP inteiro. Não execute os scripts de dentro do arquivo compactado.
3. Abra um terminal na pasta extraída, onde estão `README.md`, `install.sh`, `presets.json` e `assets/`.

### Pelo Git

Copie a URL do repositório no botão **Code** e execute, substituindo `URL_DO_REPOSITORIO` pelo endereço copiado:

```bash
git clone URL_DO_REPOSITORIO carrinho
cd carrinho
```

Git só é necessário se você escolher essa forma de baixar.

## 3. Instalar as dependências

Execute na pasta do projeto:

```bash
sh ./install-deps.sh
```

Esse script usa `sudo` apenas para instalar os pacotes do sistema com APT. A senha solicitada é a senha do seu usuário no Linux. Em uma instalação Mint/Cinnamon comum, parte dos pacotes já estará instalada.

O comando equivalente é:

```bash
sudo apt-get update
sudo apt-get install -y python3 python3-gi python3-gi-cairo python3-cairo python3-xlib gir1.2-gtk-3.0 libglib2.0-bin systemd dbus-user-session
```

Se Nemo não estiver instalado no seu ambiente Cinnamon:

```bash
sudo apt-get install nemo
```

## 4. Instalar o Carrinho

Faça login na sua sessão gráfica **Cinnamon / X11**. Em seguida, na pasta do projeto:

```bash
sh ./install.sh
```

**Não use sudo nesse comando.** O aplicativo é instalado somente para o usuário atual. O instalador precisa ser executado dentro da sessão gráfica, com o D-Bus e o systemd do usuário disponíveis.

O instalador:

1. Confere as dependências Python, o ambiente Cinnamon e o serviço do usuário.
2. Copia o código, todas as imagens e as duas pré-definições incorporadas para `~/.local/share/carrinho-y2k/`.
3. Cria a entrada **Carrinho** no menu de aplicativos.
4. Registra **Ctrl + Super + C** no Cinnamon.
5. Instala e habilita o serviço `carrinho-gestos.service`.
6. Configura o início da integração na próxima sessão.
7. Abre o carrinho para uso imediato.

`Instalar.sh` é o mesmo instalador chamado por `install.sh`. Instalações anteriores deste aplicativo são atualizadas no mesmo local. O instalador não apaga suas preferências, pré-definições pessoais nem os caminhos atualmente guardados na cesta.

## 5. Abrir e fechar

- **Menu de aplicativos → Carrinho**: abre as configurações.
- **Ctrl + Super + C**: mostra ou oculta a cesta diretamente.
- **Ícone da bandeja**: também alterna a visibilidade da cesta.
- **Símbolo de cancelar da cesta**: esvazia a cesta e encerra sua janela. Se as configurações estiverem abertas, elas permanecem abertas, com as alterações não salvas preservadas.
- **Ocultar** ou usar o atalho não esvazia a cesta.

A janela permanece acima das janelas comuns e acompanha as áreas de trabalho. Telas de bloqueio e tela cheia exclusiva podem ter prioridade sobre ela.

## 6. Juntar arquivos e pastas

1. Abra a cesta pelo atalho.
2. Arraste arquivos ou pastas para a imagem do carrinho.
3. Navegue para outra pasta e repita o gesto quantas vezes precisar.

A cesta guarda **referências aos caminhos**, sem mover ou copiar os arquivos nesse momento. Adicionar o mesmo caminho novamente não cria uma segunda entrada. A vista compacta representa até os últimos 24 itens; o contador, a grade e as transferências consideram o conjunto completo.

Se você renomear, mover ou apagar um arquivo por outro programa, o caminho salvo pode deixar de existir: remova essa referência e adicione o arquivo novamente.

## 7. Ver e remover itens

Clique no ícone de **ver itens**. Os arquivos descem para uma grade de até cinco colunas; use a rolagem quando houver mais itens.

- **× de um arquivo**: remove somente sua referência. Não apaga o arquivo original.
- **Ícone de recolher**: devolve os itens à cesta.
- **Botão direito → Esvaziar referências**: limpa toda a lista sem apagar os originais.

## 8. Copiar ou mover para uma pasta

1. Arraste a imagem da cesta para a área de arquivos de uma pasta no **Nemo**.
2. Ao soltar, aparecem botões flutuantes **Copiar**, **Mover** e **×**.
3. Escolha a operação ou cancele a entrega.

**Mover altera a localização dos arquivos originais.** Copiar preserva a origem. Nomes repetidos recebem um número, sem sobrescrever os existentes. Uma pasta adicionada já inclui seus filhos; adicionar também um filho não gera uma segunda transferência desnecessária.

Itens transferidos com sucesso saem da cesta. Os que falharem permanecem, e o aplicativo informa o erro. Não é permitido colocar uma pasta dentro dela mesma. Se o destino não aceitar XDS, use **botão direito → Entregar em uma pasta…**.

## 9. Anexos e uploads

Arraste a cesta para um aplicativo ou área de upload compatível. O aplicativo oferece os arquivos existentes como uma lista nativa de URIs `file://`.

A aceitação de vários arquivos, diretórios ou anexos depende do destino. Não há garantia de funcionar em qualquer site ou em qualquer campo de email. O aplicativo não recebe uma confirmação do término de um upload, por isso mantém os itens na cesta após esse arraste.

## 10. Abrir com uma sacudida

Enquanto estiver **arrastando arquivos com o botão esquerdo pressionado**, faça pelo menos três mudanças rápidas de direção na horizontal, com amplitude de aproximadamente 80 pixels, em menos de um segundo. A cesta aparece; continue o arraste e solte os arquivos sobre ela.

O detector exige um arraste X11 ativo com tipos de arquivo e origem reconhecida. Inclui Nemo e sua área de trabalho, além de origens como Nautilus, Thunar, Caja, PCManFM, File Roller, Engrampa, Ark, Xarchiver e PeaZip. O suporte a arquivos comprimidos depende de o gerenciador exportar os itens por arraste. O Carrinho não extrai arquivos compactados por conta própria. Essas origens adicionais não foram todas testadas individualmente.

Movimentos comuns do mouse e arrastes de texto não devem abrir a cesta. O serviço de gestos continua ativo quando você fecha a janela do carrinho.

## 11. Personalizar aparência e disposição

Abra **Carrinho** pelo menu do sistema:

- **Imagem do carrinho**: escolha uma imagem; suas proporções e margens transparentes são preservadas como na prévia.
- **Ícone de cancelar** e **ícone de ver itens**: escolha imagens independentes.
- **Ícone quando os itens estão abertos**: inverter a seta ou usar outra imagem para recolher.
- **Tamanho da imagem do carrinho**: muda apenas o tamanho da imagem principal.
- **Tamanho dos ícones de arquivos**: muda os ícones dos arquivos, inclusive na grade.
- Os dois controles de tamanho dos botões mudam somente seus respectivos ícones.
- **Inclinação aleatória**: varia a rotação dos ícones.
- **Espalhamento**: distribui os arquivos em uma área maior ou menor.
- **Sobreposição**: aproxima os centros dos arquivos para que se sobreponham mais.

Para posicionar: selecione o elemento no campo da prévia, ative **Posicionar elemento** e clique no ponto desejado da imagem.

Animações disponíveis: **deslizar**, **esmaecer**, **aproximar**, **surgir de baixo** e **sem animação**. Ajuste a duração e clique em **Testar animação** para visualizá-la. As rodas de flores do desenho original giram durante o deslizamento; imagens personalizadas não têm rodas detectadas ou animadas automaticamente.

A prévia é atualizada imediatamente. Use **Salvar alterações** para aplicar e persistir. O botão fica **azul** enquanto há alterações e **cinza / Salvo** quando não há alterações pendentes. Use a barra de título do sistema para maximizar, restaurar, minimizar e fechar a tela de opções.

## 12. Pré-definições e reset

Escolher **Carrinho de compras** ou **Steeve** aplica todas as opções salvas nessa pré-definição. Elas vêm com o programa, incluindo todas as imagens, sem depender do computador de quem preparou o projeto.

- **Salvar alterações** atualiza a cópia pessoal da pré-definição ativa.
- **Salvar nova pré-definição** pede um nome e guarda todas as imagens, os dois estados do ícone de itens, posições, tamanhos, inclinação, espalhamento, sobreposição, animação e duração.
- Trocar de pré-definição com alterações pendentes pede confirmação antes de descartá-las.
- **Restaurar padrões** devolve as opções originais e salva esses valores na pré-definição ativa. Não apaga os arquivos nem as referências na cesta.

Os padrões incorporados ficam em `presets.json`. Suas cópias editadas e suas novas pré-definições ficam no perfil do usuário; o código do programa não é reescrito ao salvar.

## 13. Arquivos instalados

| Local | Conteúdo |
| --- | --- |
| `~/.local/share/carrinho-y2k/` | Programa, imagens e pré-definições de fábrica |
| `~/.local/share/applications/carrinho-y2k.desktop` | Entrada no menu |
| `~/.config/autostart/carrinho-y2k.desktop` | Inicialização da integração |
| `~/.config/systemd/user/carrinho-gestos.service` | Serviço do usuário |
| `~/.config/carrinho-y2k/settings.json` | Configuração atual |
| `~/.config/carrinho-y2k/presets.json` | Pré-definições pessoais |
| `~/.config/carrinho-y2k/assets/` | Imagens escolhidas pelo usuário e copiadas ao salvar |
| `~/.local/state/carrinho-y2k/items.json` | Caminhos na cesta |
| `~/.local/state/carrinho-y2k/app.log` | Registro de execução |

O aplicativo não usa rede. Não envia arquivos ou configurações a um servidor. As imagens escolhidas são copiadas ao salvar para não dependerem de sua pasta original.

## 14. Atualizar

Baixe a nova versão do repositório, extraia e execute novamente:

```bash
sh ./install.sh
```

A atualização reinicia o aplicativo e o serviço; salve alterações pendentes antes. Suas pré-definições e configurações já salvas são preservadas. Em versões futuras, novas pré-definições incorporadas são acrescentadas quando ainda não existirem no seu perfil.

## 15. Diagnóstico

### O atalho não abre a cesta

Confira se instalou dentro da sessão Cinnamon/X11 e se **Configurações do Sistema → Teclado → Atalhos personalizados** contém Carrinho. Outro aplicativo pode disputar a mesma combinação.

Abra manualmente:

```bash
sh "$HOME/.local/share/carrinho-y2k/Abrir-Carrinho.sh" --show
```

Para abrir as opções:

```bash
sh "$HOME/.local/share/carrinho-y2k/Abrir-Carrinho.sh" --settings
```

### A sacudida não funciona

```bash
systemctl --user status carrinho-gestos.service
journalctl --user -u carrinho-gestos.service -n 40 --no-pager
systemctl --user restart carrinho-gestos.service
```

Confirme a sessão:

```bash
printf '%s\n' "$XDG_SESSION_TYPE"
```

Ela deve informar `x11`. O gesto precisa ocorrer durante um arraste de arquivos compatível, não apenas ao mover o cursor.

### Erro ao abrir a interface ou imagens

```bash
/usr/bin/python3 -c 'import gi, cairo, Xlib; gi.require_version("Gtk", "3.0"); print("Dependências OK")'
tail -n 50 "$HOME/.local/state/carrinho-y2k/app.log"
```

Reinstale as dependências se aparecer `ModuleNotFoundError`. Não use o Python de uma virtualenv para substituir o Python do sistema.

Se houver problema no serviço após entrar na sessão:

```bash
sh "$HOME/.local/share/carrinho-y2k/Iniciar-integracao.sh"
```

### O destino não aceita a cesta

Use uma pasta local no Nemo ou **Entregar em uma pasta…**. Áreas de texto, destinos sem XDS e sites que não aceitam múltiplos arquivos podem recusar o arraste.

## 16. Desinstalar

Na pasta deste projeto:

```bash
sh ./uninstall.sh
```

Isso remove o programa, o serviço, o autostart, o atalho e a entrada do menu. Preserva preferências e referências para uma reinstalação.

Para remover também as preferências, imagens pessoais copiadas e referências da cesta:

```bash
sh ./uninstall.sh --purge
```

**Nenhuma dessas opções apaga os arquivos originais que foram adicionados à cesta.** Os pacotes compartilhados do sistema não são removidos.

## 17. Testes e estrutura do repositório

Depois de instalar as dependências, na pasta do projeto:

```bash
/usr/bin/python3 -m unittest discover -s tests -v
```

Os testes de lógica usam diretórios temporários e não modificam suas preferências. A interface e o arraste real devem ser conferidos também numa sessão gráfica Cinnamon/X11.

```text
carrinho.py               Janela flutuante, animações e transferências
settings_ui.py           Configurações, pré-definições e persistência
watcher.py               Detector do gesto de sacudir
presets.json             Carrinho de compras e Steeve incorporados
assets/                  Arte padrão e imagens das pré-definições
install.sh               Entrada do instalador
Instalar.sh              Instalação e integração com Cinnamon
install-deps.sh          Instalação dos pacotes APT
uninstall.sh             Remoção e limpeza opcional
Abrir-Carrinho.sh         Inicializador
Iniciar-integracao.sh     Inicializador da integração na sessão
tests/                   Verificações automáticas
```

O código-fonte está sob a licença MIT, descrita em `LICENSE`. A licença de código não atribui direitos sobre imagens fornecidas separadamente; consulte `assets/README.md`.


## 18. Publicar esta versão no GitHub

O pacote está pronto para um repositório, mas não inclui uma conta, URL ou credencial do GitHub.

1. Crie um repositório vazio no GitHub com o nome desejado.
2. Coloque todos os arquivos desta pasta na raiz desse repositório, incluindo `assets/`, `presets.json`, os scripts e `.github/`.
3. Não inclua seu `~/.config/carrinho-y2k/`, os arquivos da cesta ou seus registros pessoais.

Se preferir publicar pelo terminal, dentro da pasta do projeto, depois de criar o repositório vazio e copiar sua URL:

```bash
git init -b main
git add .
git commit -m "Versão inicial do Carrinho"
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```

Substitua `URL_DO_REPOSITORIO` pela URL real do repositório que você criou. O Git pode solicitar sua identificação de autor, e o envio exige autenticação do GitHub. O fluxo de verificações em `.github/workflows/check.yml` executa os testes de lógica a cada push ou pull request; ele não substitui o teste gráfico no Cinnamon/X11.
