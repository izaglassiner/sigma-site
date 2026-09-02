# Tutorial: Workflow Ortomosaico Completo

Este tutorial ensina, passo a passo, como configurar uma máquina nova (Windows, Linux ou macOS) para rodar o pipeline do projeto **SIGMA** e gerar o ortomosaico do Espírito Santo a partir do script [`run_example.py`](https://github.com/fboldt/sigma/blob/main/run_example.py).

Repositório: [https://github.com/fboldt/sigma](https://github.com/fboldt/sigma)

---

## 1. Pré-requisitos

Antes de começar, você precisa ter instalado na sua máquina:

- **Python 3.10 ou superior** — [baixar aqui](https://www.python.org/downloads/)
  - No Windows, durante a instalação, marque a opção **"Add Python to PATH"**.
  - Para verificar se o Python já está instalado, abra o terminal e rode:
    ```bash
    python --version
    ```
    ou, em alguns sistemas Linux/Mac:
    ```bash
    python3 --version
    ```
- **Git** instalado — [baixar aqui](https://git-scm.com/downloads)
  - Para verificar, rode:
    ```bash
    git --version
    ```
- Um **cadastro ativo na plataforma do INPE** (necessário para baixar as cenas do satélite CBERS-4A usadas pelo `run_example.py`). Caso ainda não tenha, crie sua conta em [https://data.inpe.br](https://data.inpe.br).

---

## 2. Clonando o repositório

Abra o terminal (Prompt de Comando, PowerShell, Terminal do Linux/Mac) e navegue até a pasta onde deseja salvar o projeto. Em seguida, rode:

```bash
git clone https://github.com/fboldt/sigma.git
```

Depois, entre na pasta do projeto:

```bash
cd sigma
```

---

## 3. Criando o ambiente virtual (venv)

O ambiente virtual mantém as dependências do projeto isoladas do restante do sistema. Crie e ative a venv de acordo com o seu sistema operacional.

### Windows (PowerShell ou CMD)

Criar a venv:
```powershell
python -m venv venv
```

Ativar a venv (PowerShell):
```powershell
venv\Scripts\Activate.ps1
```

Ativar a venv (CMD):
```cmd
venv\Scripts\activate.bat
```

> Se o PowerShell bloquear a execução do script com um erro de política de execução, rode antes:
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
> ```

### Linux

Criar a venv:
```bash
python3 -m venv venv
```

Ativar a venv:
```bash
source venv/bin/activate
```

### macOS

Criar a venv:
```bash
python3 -m venv venv
```

Ativar a venv:
```bash
source venv/bin/activate
```

Em todos os casos, depois de ativada, o nome `(venv)` deve aparecer no início da linha do terminal, indicando que o ambiente virtual está ativo.

---

## 4. Instalando as dependências (requirements)

Com a venv **ativada**, instale todas as bibliotecas necessárias listadas no `requirements.txt`:

```bash
pip install -r requirements.txt
```

Esse comando é o mesmo para Windows, Linux e macOS — o importante é que a venv esteja ativa antes de rodá-lo.

> Caso o comando `pip` não seja reconhecido, tente `pip3` no lugar dele.

---

## 5. Configurando o script antes de rodar

Antes de executar, abra o arquivo `run_example.py` (na raiz do repositório) em um editor de texto e localize a linha:

```python
user = 'email@email.com'  # E-mail cadastrado na plataforma do INPE
```

Substitua `'email@email.com'` pelo **e-mail que você cadastrou na plataforma do INPE** (passo 1). Sem isso, a busca e o download das cenas de satélite não vão funcionar.

Você também pode ajustar, se quiser, o intervalo de datas (`initial_date` e `final_date`) e a cobertura máxima de nuvens (`max_cloud`) definidos no mesmo arquivo, mas os valores padrão já são suficientes para reproduzir o mosaico do Espírito Santo.

---

## 6. Rodando o código com PYTHONPATH

O script `run_example.py` importa módulos da pasta `utils/` (como `utils.search`, `utils.download`, `utils.mosaic`, etc.), então é preciso garantir que a raiz do projeto esteja no `PYTHONPATH` antes de rodar. Com a venv ativa e dentro da pasta `sigma`, use o comando correspondente ao seu sistema operacional:

### Windows (PowerShell)

```powershell
$env:PYTHONPATH="."; python run_example.py
```

### Windows (CMD)

```cmd
set PYTHONPATH=. && python run_example.py
```

### Linux

```bash
PYTHONPATH=. python3 run_example.py
```

### macOS

```bash
PYTHONPATH=. python3 run_example.py
```

O processamento pode levar bastante tempo, dependendo da quantidade de cenas encontradas para o Espírito Santo e da velocidade da sua conexão com a internet (o script busca, filtra e baixa todas as cenas CBERS-4A disponíveis no período configurado).

---

## 7. Resultado esperado

Durante a execução, o terminal vai exibir mensagens indicando o progresso de cada etapa do workflow, por exemplo:

```
Iniciando busca de produtos das bandas.
Iniciando filtragem de produtos retornados em um mesmo local.
Iniciando download das bandas.
Download finalizado! Arquivos salvos em: ./images
Iniciando composição RGB.
Composição finalizada! Arquivos salvos em: ./images/TRUE_COLOR
Iniciando formação do mosaico.
Processo concluído! Mosaico salvo em: ./images/NOVO_MOSAICO_SPECTRALMATCH
```

Ao final, dentro da pasta `sigma/images`, você deve encontrar:

- As bandas baixadas de cada cena do satélite;
- As composições RGB de cada cena, na subpasta/arquivos `TRUE_COLOR`;
- O **ortomosaico final do Espírito Santo**, salvo como `NOVO_MOSAICO_SPECTRALMATCH`.

Esse arquivo final é o mosaico contínuo e georreferenciado do estado, gerado a partir da união e do ajuste radiométrico de todas as cenas baixadas.

---

## 8. Encerrando a venv (opcional)

Quando terminar de usar o projeto, você pode desativar o ambiente virtual com:

```bash
deactivate
```

O comando é o mesmo em Windows, Linux e macOS.
