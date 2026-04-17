# Instalação de Pacotes `.tar.gz` no Linux


### Pré-requisitos

Antes de começar, certifique-se de ter as ferramentas de build instaladas. Sem isso, a compilação vai falhar na metade do caminho.

No Debian/Ubuntu/Mint:

	sudo apt install build-essential

No Arch/Manjaro:

	sudo pacman -S base-devel

No Fedora/RHEL:

	sudo dnf groupinstall "Development Tools"

Verifique se o `make` e o `gcc` estão disponíveis:

	make --version
	gcc --version


### Extraindo o arquivo

Navegue até o diretório onde o arquivo foi baixado e extraia:

	tar -xzf pacote.tar.gz

Isso cria um diretório com o nome do pacote. Entre nele:

	cd nome-do-pacote/

Caso não saiba o nome exato do diretório criado:

	ls -lh

Para extrair em um diretório específico:

	tar -xzf pacote.tar.gz -C /caminho/destino/

Para apenas listar o conteúdo sem extrair:

	tar -tzf pacote.tar.gz


### Lendo a documentação do pacote

Antes de qualquer coisa, leia os arquivos de instrução que quase todo pacote inclui:

	ls

Procure por arquivos como `README`, `INSTALL`, `README.md` ou similares. Eles podem indicar dependências específicas ou um processo de instalação diferente do padrão.

	cat README
	cat INSTALL


### Configurando o pacote

A maioria dos pacotes compilados manualmente usa o script `./configure` para verificar dependências e preparar o ambiente. Verifique se ele existe:

	ls configure

Se existir, rode:

	./configure

Caso queira instalar em um diretório diferente do padrão (`/usr/local`), use o parâmetro `--prefix`:

	./configure --prefix=/opt/meu-programa

Se o `./configure` falhar com erro de dependência faltando, instale o pacote indicado pelo erro e rode novamente.

Alguns pacotes mais modernos usam `CMake` no lugar do `./configure`. Se não houver script `configure` mas houver um `CMakeLists.txt`:

	mkdir build
	cd build
	cmake ..


### Compilando

Com a configuração concluída, compile o código-fonte:

	make

Esse processo pode demorar dependendo do tamanho do pacote e do seu hardware. Para usar múltiplos núcleos e acelerar a compilação:

	make -j$(nproc)

Se ocorrerem erros durante a compilação, geralmente indicam dependências faltando. Instale o que for indicado e rode `make` novamente. Se precisar recomeçar do zero:

	make clean
	./configure
	make


### Instalando

Com a compilação concluída, instale o programa no sistema:

	sudo make install

Por padrão, os arquivos são colocados em `/usr/local/bin`, `/usr/local/lib`, etc. Se tiver usado `--prefix` no passo anterior, os arquivos serão instalados no diretório especificado.

Verifique se a instalação foi bem-sucedida:

	which nome-do-programa
	nome-do-programa --version


### Desinstalando

Se o pacote suportar desinstalação via `make` (a maioria suporta):

	sudo make uninstall

Caso o comando não esteja disponível, será necessário remover os arquivos manualmente. O `make install` geralmente lista os arquivos copiados durante a instalação, então revisar o output pode ajudar a localizar o que precisa ser removido.


### Referência rápida de flags do `tar`

| Flag | Função |
|------|--------|
| `-x` | Extrair |
| `-z` | Descompactar gzip (`.gz`) |
| `-j` | Descompactar bzip2 (`.bz2`) |
| `-J` | Descompactar xz (`.xz`) |
| `-f` | Especificar o arquivo |
| `-t` | Listar conteúdo sem extrair |
| `-v` | Modo verboso (mostra arquivos sendo extraídos) |
| `-C` | Extrair em diretório específico |

Para pacotes `.tar.bz2` ou `.tar.xz`, o processo é idêntico. Basta trocar a flag de descompressão ou deixar o `tar` detectar automaticamente:

	tar -xf pacote.tar.bz2
	tar -xf pacote.tar.xz
