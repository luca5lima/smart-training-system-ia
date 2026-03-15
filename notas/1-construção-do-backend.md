# 🏗️ Construção do Back-end: Lógica e Persistência

Nesta aula, avançamos da configuração inicial para a implementação das regras de negócio do Fit.AI, focando na criação de planos de treino complexos e na integridade dos dados.

## 🏗️ Arquitetura de Casos de Uso (Use Cases)

Para manter o código limpo e desacoplado, implementei o padrão de Use Cases. O arquivo `CreateWorkoutPlan.ts` encapsula toda a lógica necessária para **criar um novo plano de treino**.

- **DTO (Data Transfer Object)**: Utilizei interfaces para definir exatamente quais dados a aplicação deve receber, garantindo tipagem forte desde a entrada.
- **Mapeamento Hierárquico**: A lógica de criação percorre três níveis: Plano -> Dias -> Exercícios, utilizando o `create` aninhado do Prisma.

## 🔗 Singleton do Prisma e Adapters

No arquivo `src/lib/db.ts`, configurei a conexão com o banco de dados utilizando um padrão Singleton.
```ts
    import { PrismaPg } from "@prisma/adapter-pg";

    import { PrismaClient } from "../generated/prisma/client.js";

    const connectionString = `${process.env.DATABASE_URL}`;

    const adapter = new PrismaPg({ connectionString });

    const globalForPrisma = global as unknown as { prisma: PrismaClient };

    export const prisma = globalForPrisma.prisma || new PrismaClient({ adapter });

    if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;
```
- **Prevenção de Exaustão de Conexões***: No ambiente de desenvolvimento, o `tsx --watch` reinicia o servidor constantemente. O Singleton garante que não sejam criadas novas instâncias do `PrismaClient` a cada recarregamento.
- **PrismaPg Adapter**: Implementei o adaptador `@prisma/adapter-pg` para uma comunicação mais eficiente com o PostgreSQL.
- Edite o arquivo `auth.ts` tambem.

## 🧪 Transações Atômicas e Integridade

Uma das implementações mais críticas foi o uso de Transactions (`$transaction`).

- **Atomicidade**: Garanti que a desativação do plano antigo e a criação do novo ocorram como uma única unidade de trabalho. Se a criação do plano falhar, o plano antigo não é desativado.
- **Lógica de Plano Ativo**: Implementei uma verificação que busca planos ativos existentes e os marca como `isActive: false` antes de persistir o novo treino gerado pela IA.

## ⚠️ Tratamento de Erros Customizados

Criei uma estrutura base para erros da aplicação no arquivo `errors/index.ts`.

- **NotFoundError**: Classe customizada que estende `Error`, permitindo que o Fastify capture falhas de busca (como quando um plano recém-criado não é retornado) e responda com o status HTTP correto (404).