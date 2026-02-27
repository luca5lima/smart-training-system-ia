# 🛠️ Log de Engenharia #Aula0: Setup do Ambiente e Estrutura Base

Nesta etapa inicial, foquei na preparação do ambiente de desenvolvimento para garantir uma base sólida, performática e tipada para o **Fit.AI**. Optei pelo ecossistema moderno de Node.js com TypeScript e `pnpm`.

## ⚙️ Stack de Desenvolvimento
- **Gerenciador de Versão:** [NVM-Windows](https://github.com/coreybutler/nvm-windows) (Node v24.x)
- **Gerenciador de Pacotes:** `pnpm` (pela eficiência em disco e velocidade)
- **Linguagem:** TypeScript 5.9.3 (Strict Mode)
- **Runtime de Desenvolvimento:** `tsx` para execução rápida sem transpilação manual.

## 🚀 Passo a Passo da Configuração
### 1. Gerenciamento de Versão com NVM
Para evitar conflitos de versões entre diferentes projetos, utilizei o nvm-windows para instalar e gerenciar o Node.js.
Isso garante que o projeto utilize o Node v24, versão recomendada para as funcionalidades de IA que iremos implementar.
[Site](https://github.com/coreybutler/nvm-windows/releases) [Video](https://www.youtube.com/watch?v=zKTAYbcHob0)
- Instalação da versão específica: 
    ```bash
        nvm install 24
    ```
- Ativação da versão para o projeto:
    ```Bash
        nvm use 24
    ```
- Ver qual a versão do node
    ```bash
        node -v
    ```

Instale o `pnpm` no **windows**. [Site](https://pnpm.io/pt/installation)
```bash
    Invoke-WebRequest https://get.pnpm.io/install.ps1 -UseBasicParsing | Invoke-Expression
```
> Apois isso, feche e abra novamente para carregar as variaveis de ambiente

![type](../img/1.1-pnpm-install.PNG)

### 2. Inicialização do Projeto
Iniciei o projeto com o `pnpm` para gerar o `package.json` base.
```bash
    pnpm init
```
![type](../img/1-pnpm-init.PNG)

### 3. Configuração do TypeScript e Tipagem
Instalei o TypeScript e as definições de tipo para o Node.js como dependências de desenvolvimento.

```Bash
    pnpm add typescript@5.9.3 @types/node@24 -D
```
![type](../img/2-typescript.PNG)

### 4. Ferramentas de Qualidade de Código (Linting & Formatting)
Para manter o código padronizado e evitar erros comuns, configurei o ESLint e o Prettier.

```Bash
    pnpm add prettier@3.8.1 eslint@9.39.2 -D
```
![bibliotecas](../img/3-bibliotecas.PNG)

### 5. Inicialização do `tsconfig.json`
Gerei o arquivo de configuração do compilador TS com foco em performance (`target: es2024`) e rigor técnico (`strict: true`).

```Bash
    npx tsc --init
```

> Copie e cole esse código no `tsconfig.json`:
```json
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "compilerOptions": {
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "target": "es2024",
    "skipLibCheck": true,
    "strict": true,
    "outDir": "./dist"
  },
  "include": ["src"]
}
```
![tsconfig](../img/4-tsconfig.PNG)

### 6. Configurar o Servidor de Desemvolvimento
```bash
    pnpm add tsx @4.21.0
``` 

### 7. Padronização de Ambiente e Motores (Engines)
Para garantir que todos os desenvolvedores utilizem exatamente a mesma versão do Node.js, configurei uma trava de segurança no projeto.

- Definição de Engines: No `package.json`, adicionei o campo `"engines"` especificando a versão `24.x` do Node.
- Configuração do `.npmrc`: Criei o arquivo `.npmrc` com a instrução `engine-strict=true`. Isso força o gerenciador de pacotes a interromper a instalação caso a versão do Node esteja incorreta.
- Validação na Prática: Realizei um teste tentando usar o Node `v22.20.0`, o que resultou no erro `ERR_PNPM_UNSUPPORTED_ENGINE`, comprovando que a trava está funcionando.

![tsconfig](../img/7-engines-npmrc.PNG)